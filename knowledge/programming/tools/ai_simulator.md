---
type: knowledge
domain: programming/tools
last_updated: 2026-05-19
status: built (実機APIテスト未実施)
---

# ai-simulator — 複数AIペルソナ並列シミュレーター (セッションη)

> YD のコミュニケーション/リーダーシップを「複数AIキャラに同時に話しかけられる状況」で鍛える練習環境。
> 例: Salamat代表として10人のメンバーから一斉に質問された状況をAI 10体で再現、回答スキルを実戦形式で鍛える。
>
> 作成: 2026-05-19 (セッションη、Claude Code)
> パス: `~/projects/ai-simulator/`

---

## ⚡ 1分でわかる仕様

- **ペルソナ数**: 5カテゴリ × 計30バリエーション (Salamatメンバー / Apple顧客 / Arte Grow現地パートナー / 映像クライアント / 面接官)
- **シナリオ数**: 4本
  - `salamat_team_chaos` (extreme, 10人質問攻め)
  - `apple_sales_rush` (hard, 混雑時5人同時接客)
  - `crisis_management` (extreme, 視察直前トラブル7人)
  - `client_pitch` (hard, Arte Grow 商談4人)
- **モデル**: Sonnet 4.6 推奨 (品質優先)、`--budget` で Haiku 4.5 切替
- **コスト**: 1セッション $0.30〜$0.70 想定、`config.yaml` の `cost_cap_usd: 1.0` でハード上限
- **ログ**: `logs/<session_id>.jsonl` (機械可読) + `.md` (人間可読)
- **振り返り**: 終了時に Claude が評価ルーブリックに沿って Markdown レポートを自動生成し、`~/ObsidianVault/learning/simulations/<session_id>.md` に保存
- **入力 UX**: broadcast / `@名前 個別宛` / `/tick` (AI自発発話) / `/quit` (終了)

## ディレクトリ構造

```
ai-simulator/
├── personas/                   # ペルソナYAML 5種 × 計30 variants
├── scenarios/                  # シナリオYAML 4種
├── src/ai_simulator/
│   ├── engine/
│   │   ├── types.py            # Pydantic モデル
│   │   ├── persona_loader.py
│   │   ├── scenario_loader.py
│   │   ├── conversation_log.py
│   │   ├── cost.py             # トークン→USD換算 (Sonnet/Haiku 価格表)
│   │   ├── orchestrator.py     # asyncio 並列 API + prompt cache
│   │   └── reflection.py       # 振り返りレポート生成
│   ├── interface/cli.py        # Typer + rich
│   ├── __main__.py
│   └── main.py
├── tests/                      # ユニットテスト 19件 (オフラインで全通る)
├── config.yaml
├── pyproject.toml              # uv 管理、deps: anthropic / rich / typer / pyyaml / pydantic
├── .env.example
└── README.md
```

## セットアップ (YD作業)

```bash
cd ~/projects/ai-simulator
uv sync                                    # 既に1回完了
cp .env.example .env                       # ANTHROPIC_API_KEY を埋める
uv run ai-simulator list                   # シナリオ一覧表示
uv run ai-simulator run salamat_team_chaos # 起動
```

## 使用感 (CLI仕様)

```
YD> こんにちは、一旦止まって順に話していきましょう       # 全員に broadcast
YD> @リコ アポは取れてない、来週中に取る                # リコだけ
YD> /tick                                              # AIが自発発話
YD> /who                                               # 参加者一覧
YD> /cost                                              # 現在の推定コスト
YD> /quit                                              # 終了 → 振り返り生成
```

ターン中、各ペルソナはそれぞれの色で名前+発話が表示される。
prompt cache (ephemeral) でシステムプロンプトをキャッシュし、2ターン目以降のトークンを節約。

## ペルソナ追加方法

`personas/<id>.yaml` を新規作成。テンプレ:

```yaml
id: my_persona
display_name: "わかりやすい名前"
description: "..."
context: |
  共通の世界観
prompt_template: |
  あなたは「{name}」を演じます。
  - 役割: {role}
  - 性格: {personality}
  - 話し方: {speaking_style}
  - 背景: {backstory}
  - 関心: {concerns}
  ## 共通の前提
  {context}
  ## 応答ルール
  (絶対ルールをここに)
variants:
  - id: v1
    name: "個体名"
    role: ...
    personality: ...
    speaking_style: ...
    concerns: ...
    backstory: ...
```

シナリオ側で `persona: my_persona / variant: v1` で参照できる。

## シナリオ追加方法

`scenarios/<id>.yaml` を新規作成:

```yaml
id: my_scenario
name: "..."
description: "..."
difficulty: hard           # easy / medium / hard / extreme
timing: simultaneous       # / sequential / random
duration_turns: 6
turn_pace: "user_driven"
opening_scene: |
  場面説明
participants:
  - persona: salamat_member
    variant: enthusiast
    opening: "最初の発話 (省略可、placeholder の場合は ( ) で始める)"
success_criteria:
  - "成功条件1"
evaluation_rubric: |
  5観点 × 5点 = 25点満点
  1. ...
```

## 想定コスト

| シナリオ | 参加者 | 推奨ターン | 推定コスト (Sonnet 4.6) |
|---------|--------|----------|---------------------|
| salamat_team_chaos | 10 | 6 | $0.30〜$0.70 |
| apple_sales_rush | 5 | 5 | $0.15〜$0.30 |
| crisis_management | 7 | 6 | $0.25〜$0.50 |
| client_pitch | 4 | 7 | $0.15〜$0.30 |

ハード上限は `config.yaml` の `cost_cap_usd: 1.0` で強制終了 (それ以前に `warn_threshold_usd: 0.7` で警告)。

## テスト

```bash
uv run --extra dev pytest tests/ -v
```

19件 全 PASS (オフライン)。テスト対象:
- ペルソナ・シナリオの YAML ロード + 全 variant 解決
- prompt template の変数置換
- build_participants が10人正しく初期化
- `find()` の名前解決 (@prefix 対応)
- ConversationLog の append
- CostTracker の価格計算 (Sonnet/Haiku の公開価格と一致)
- 50発話シナリオが $1 以下に収まる予算試算

## 関連

- 仕様書: YD からの「セッションη」指示書 (会話内のみ)
- 自走モード: エラーに3回まで自力対処、設計判断は自分で決めて進行
- 関連プロジェクト: [[ai_researcher]] (24h研究員、別系統) / [[morning_briefing]] (朝ブリーフィング、Max20x完結) / [[claude_code]]

---

## ✅ うまく行ったこと

- **uv + src layout** で他プロジェクト (vidkit / morning-briefing) と規約を完全に揃え、`uv sync` 一発 + `uv run ai-simulator` で動作。学習コスト最小。
- **1ペルソナファイル = 複数 variants** の設計が当たり。10人シナリオを 10 ファイルに散らさず、1 ファイル内で対比が見える形で構造化できた (例: `salamat_member.yaml` 10 variants で「積極/不安/消極/挑戦/穏やか/不満/留学生/OB/会計/書記」)。
- **asyncio.gather + prompt cache** で10ペルソナ並列呼び出しを設計通り実装。各ペルソナのシステムプロンプトを `cache_control: ephemeral` で渡しているため、2ターン目以降のトークンが大幅節約 (理論上)。
- **コスト見積りをユニットテストで保証**。`test_realistic_session_under_budget` で「50発話 (10人×5ターン) で $1 以下」を回帰防止。価格表 (Sonnet 4.6 / Haiku 4.5) も `tests/test_cost.py` で公開価格と一致を検証。
- **振り返り機能を分離設計**。`engine/reflection.py` 単独で Claude に評価ルーブリック付きで会話全体を渡す形にしたので、後から評価基準を変えても他コンポーネントを触らなくて済む。
- **Vault 連携を疎結合**。`save_reflection_to_vault()` は vault_dir の親が存在しなければ no-op、存在すれば書き込む。Vault がなくても CLI は完走する。
- **ペルソナ設定にYDの実体験を密に注入**。Salamat (260名・18年・セブ/マニラ・Save Smile・Give and Take)、Apple (iPhone日本1位)、Arte Grow (Type B / Pride Not Dependency)、Yitao Film (相手主体・大川優介系)、就活 (JICA第一志望) 等を context / variant の backstory に埋めた → ペルソナの発話が一般的になりすぎない。

## ❌ 詰まったこと

- **ANTHROPIC_API_KEY が見つからなかった**。`~/projects/ai-researcher/.env` には行はあるが値は空、Keychain にも未保存、シェル環境変数にも未設定。Claude Code は OAuth 経由で動いており、API キーは別途必要。**実機での 10ペルソナ同時呼び出しは未実施** (YD 自身で `.env` 設定後に走らせる前提)。回避: オフラインで通る 19 件のユニットテストでロジックを保証。
- **Anthropic SDK の `message` 配列は role が user / assistant 交互必須**。最初に「場面情報 (user)」だけ入れて API を呼ぶと、続いて user メッセージを送る場面で連続 user になりエラー。解決: 各 participant の `opening` がプレースホルダ (`(後で発言予定)`) の場合、`{"role": "assistant", "content": "(まだ発言を控えて、状況を見ています)"}` を擬似発話として挿入し、サイクルを保つ。
- **broadcast と @mention のハイブリッド時に user 連続が発生し得る**。`mention()` で他ペルソナに観察情報を user として注入 → 直後の broadcast でまた user が来ると連続。解決: `_ask()` の中で「履歴末尾が user なら user_text にマージする」ロジックを入れた (`orchestrator.py:_ask`)。
- **rich の `Live` 表示は asyncio.gather + stdin input と相性が悪い**。今回は `console.print()` の単純な逐次表示にして、`asyncio.to_thread(input, "YD> ")` でブロッキング input を非同期化することで回避。
- **完了条件「実機 salamat_team_chaos で動作確認」が達成できなかった**。YD と相談して「ユニットテスト + コード完成」で止め、実機検証は YD 側に委譲する判断 (AskUserQuestion で確認、YD 承諾)。

## 📋 次回同じことをするときのチェックリスト

### 起動前 (YD作業)

1. `cd ~/projects/ai-simulator && cp .env.example .env`
2. `.env` の `ANTHROPIC_API_KEY` を埋める (もしくは `export ANTHROPIC_API_KEY=...` を `.zshrc` に)
3. `uv sync` (初回のみ)
4. `uv run ai-simulator list` でシナリオ一覧が出ることを確認
5. (任意) Vault 振り返り保存先 `~/ObsidianVault/learning/simulations/` を `mkdir -p` で作っておくと初回保存がスムーズ (なくても CLI が自動作成)

### 初回プレイ時のチェック

1. 軽い方から: `uv run ai-simulator run client_pitch --budget` (Haiku で 1回 $0.05 程度)
2. UI の挙動を確認: broadcast / `@名前` / `/tick` / `/cost` / `/quit`
3. `/cost` で実コストが想定内か確認 (Sonnet で 1セッション $0.3〜$0.7 想定)
4. 終了後、`logs/<session_id>.md` と Vault `learning/simulations/<session_id>.md` を開いて振り返りを読む
5. 振り返りスコア (0〜25) を見て、次回どこを意識して捌くか決める

### ペルソナ・シナリオを増やす時

1. `personas/<id>.yaml` を作成 → variants を3つ以上 (テストの最低数)
2. `prompt_template` に必ず `{name}` を含める (`test_each_persona_loads_and_has_variants` のチェック対象)
3. `scenarios/<id>.yaml` の participants は 必ず実在の persona × 実在の variant を指す (`test_scenario_participant_variants_resolve` でチェック)
4. `success_criteria` と `evaluation_rubric` を埋める (振り返りで Claude が参照)
5. `uv run --extra dev pytest tests/` でロード系テストが通ることを確認

### コスト超過しそうな時

- `--budget` で Haiku 4.5 に切替 (1/3 程度のコスト)
- `config.yaml` の `model.max_tokens_per_utterance` を 220 → 150 に下げる
- `session.cost_cap_usd` で強制終了ラインを下げる (デフォ $1.0)
- 参加者を10人 → 7人に減らす (シナリオYAMLを編集)

### 落とし穴

- ペルソナの prompt_template に `{name}` 以外で `{...}` を使うと `str.format` で例外。値置換しない `{...}` は `{{...}}` でエスケープ。
- 同じ variant を複数 instance で並べたいときは、現状コード上は問題ないが内部状態 (history) が独立しているか必ず単体テストで確認すること。
- `vault_simulations_dir` を相対パスで書くと CLI 起動ディレクトリ依存になる → 絶対パス (`/Users/ittou/ObsidianVault/...`) を推奨。
