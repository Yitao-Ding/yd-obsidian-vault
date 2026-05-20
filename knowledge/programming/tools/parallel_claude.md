---
type: knowledge
domain: programming/tools
created: 2026-05-20
last_updated: 2026-05-20
tags: [claude-code, parallel, nohup, cron, monitoring, max20x]
priority: medium
---

# parallel-claude — 並列 Claude Code 監視基盤

> YD が寝てる間、または別作業中、複数の `claude --print` セッションを並列でバックグラウンド実行し、5分ごとに親 Claude が状態を集約する基盤。Max 20x 枠完結で API 課金ゼロ。

初版作成: 2026-05-20 03:15 (YD「全部監視して、寝るから」依頼)
パス: `~/projects/parallel-claude/`

## 🎯 何をするツールか

- N 個の `claude --print --dangerously-skip-permissions` セッションを `nohup` でバックグラウンド起動
- 親 Claude Code が `CronCreate` で 5分おきに `monitor.sh` を叩く
- `monitor.sh` は: PID 生存 / DONE.txt 生成 / 外部 `_pids.txt` 取り込み / `ps aux` 新規検出 を行い、結果を Vault に追記
- 全完了で `CronDelete` 自殺、ループ終了

## 🏗 構成

```
~/projects/parallel-claude/
├── README.md
├── state/
│   └── sessions.json       全セッション状態 (PID, status, log_file, ...)
├── tasks/                  各セッションの prompt
│   ├── 01_<name>.md
│   └── ...
├── logs/                   stream-json 出力 + <name>_DONE.txt / _FAIL.txt
└── scripts/
    ├── launch_session.sh   1セッション起動 (汎用)
    ├── launch_all.sh       全タスクを順次起動
    ├── monitor.sh          状態スキャン
    └── status_report.sh    整形出力
```

### state/sessions.json スキーマ

```json
{
  "version": 1,
  "created_at": "ISO8601",
  "sessions": [
    {
      "id": "01_vault_integrity",
      "task_file": "tasks/01_vault_integrity.md",
      "pid": 12345,
      "started_at": "ISO8601",
      "log_file": "logs/01_vault_integrity_<ts>.jsonl",
      "status": "running | completed | failed | died",
      "exit_code": null,
      "completed_at": null,
      "discovered_by_monitor": false,
      "external_source": "business-plan-sprint",   // optional
      "command_snippet": "claude -p ..."           // optional (ps 検出時)
    }
  ],
  "last_monitor_check": "ISO8601",
  "monitor_iterations": 12
}
```

## 🚀 起動方法

### 1. タスク prompt を書く

`tasks/<NN>_<name>.md` に各セッションの指示。必須要素:

- スコープ (やること / やらないこと)
- 出力先パス
- 完了条件 (`logs/<id>_DONE.txt` に書く)
- 失敗時 (`logs/<id>_FAIL.txt`)
- 「YD は寝てる、承認得られない」の明示
- 終了直前に `~/ObsidianVault/log.md` に1行追記

### 2. 起動

```bash
~/projects/parallel-claude/scripts/launch_all.sh
```

### 3. 監視ループを CronCreate で登録

親 Claude Code セッションで:

```text
CronCreate(
  cron="2-59/5 * * * *",       # 5分ごと、:00 集中回避
  recurring=true,
  prompt="parallel-claude 監視ターン... (詳細省略)"
)
```

### 4. 自然終了

全 `running=0` になったら、prompt 内で `CronDelete` を呼んで自分自身を停止。

## 👁 監視ループ (`monitor.sh`)

各 iteration で:

1. **既存セッション状態チェック** (`kill -0` + DONE/FAIL マーカー)
2. **外部 `_pids.txt` 取り込み** — 別系統スクリプト (例: `business-plan-sprint`) の PID ファイルを読んで自分の state に統合
3. **`ps aux` 新規プロセス検出** — `(\s|^)(-p|--print)(\s|$)` で claude CLI を検出、未登録なら追加
4. `~/ObsidianVault/raw/monitoring/YYYY-MM-DD.md` に詳細追記
5. `~/ObsidianVault/log.md` に1行サマリ

### 出力例

```
iter=8 running=3 done=5 fail=22 discovered_new=0
```

## 🔌 外部 _pids.txt 連携

`monitor.sh` の `EXTERNAL_PID_SOURCES` リストに別系統の `_pids.txt` パスを登録すると、それも統合監視される。フォーマット:

```text
<session_name> PID=<pid> LOG=<log_path>
```

業務システム的に他の並列ランチャー (`business-plan-sprint`, `morning-briefing` の launch.sh など) がある場合、それぞれが `_pids.txt` を吐けば一括監視可能。

## 💰 コスト

- LLM 経路: `claude --print` (OAuth、Max 20x 枠内)、`apiKeySource:"none"` 確認済
- TTS / API: なし
- **実支払い $0** (Sonnet 換算で 1 セッション $0.3-0.7 相当、参考値)

## 🚦 セキュリティ / 安全装置

`--dangerously-skip-permissions` でも、子セッションの暴走を抑える設計レイヤー:

- 各タスク prompt で「スコープ外のことはしない」を明示
- システムプロンプト (`--append-system-prompt`) で「他セッションの作業領域に書き込まない」を強制
- 完了マーカー (`DONE.txt`) で「やったこと」の証跡を残す
- YD 完全バイパス指示時は、子に `git push / rm -rf / sudo` 禁止を入れる (今回は省略)

---

## ✅ うまく行ったこと

### 初回運用 (2026-05-20 深夜)

- **Max 20x 完結**: 17セッション 1時間で $0 (Sonnet 換算で $5-10 相当)、API 課金ゼロ
- **`ps aux` 自動検出**: YD が別ターミナルで起動した BPS 12本 (3:13起動分 + 3:23再起動分) を全部統合監視できた
- **`_pids.txt` import**: 別系統スクリプトとの統合がシンプル (1ファイル読むだけ)
- **CronCreate session-only でも十分**: 7日持つので寝てる間の 4-5 時間は余裕
- **JSONL stream 出力**: 死亡時の原因 (API socket error, ScheduleWakeup 誤用) が即座に判明
- **monitor.sh の `monitor_iterations` カウンタ**: イテレーションごとの状態スナップショットが Vault に残る → 後追い解析が楽

## ❌ 詰まったこと

### 初回運用で踏んだ罠

- **UTF-8 strict decode**: `subprocess.run(text=True)` が `tail -3` のマルチバイト境界で死ぬ。`capture_output=True` で bytes 受け取り → `.decode('utf-8', errors='replace')` に書き換え。詳細: [[claude_mistakes]] A-11
- **`ps aux` 検出プロセスの状態判定**: PID が死んだら問答無用で `died` にしてた。本来は `log_size > 0` なら `completed` 判定すべき。詳細: [[claude_mistakes]] A-12
- **prompt が引数渡しで長すぎると問題**: macOS の `ARG_MAX = 1MB` なので 5KB のタスク prompt はOK、ただし将来 100KB を超える prompt の場合は `--system-prompt-file` のようなオプションを検討
- **session_id を `--session-id <uuid>` で固定してない**: `~/.claude/projects/<cwd>/<uuid>.jsonl` に残るが UUID 不明だと resume が手間。今回は `--no-session-persistence` も入れてない (= disk に残る) ので、後で resume したければ可能だが、現状 ID 未把握

### 設計上の留意

- **二重起動の自動検出は未実装**: 別 Claude セッションが同じ launch.sh を叩いた場合、`_pids.txt` が上書きされて旧 PID が忘れられる。`_pids_history.txt` (append-only) を本家側に追加させると良い
- **「YD は寝てる」明示が無いと子が確認質問で死ぬ**: BPS の `14_hatachi_video` が `Q1. ~/Downloads/...が見当たりません` で停止した実例あり。タスク prompt の必須要素にした

## 📋 次回同じことをするときのチェックリスト

### 起動前

- [ ] 既存プロセス確認: `ps -axo pid,etime,command | grep 'claude.*-p'`
- [ ] 別系統の `_pids.txt` / log ディレクトリの存在確認 (二重起動回避)
- [ ] 各タスク prompt に「YDは寝てる/承認得られない/DONE.txtを書いて終了」を明示
- [ ] スモークテストで `apiKeySource:"none"` (= Max 20x 完結) を確認
- [ ] `claude --version` でバージョン記録 (今回 v2.1.144)

### 監視中

- [ ] `subprocess.run` でテキストは bytes 受け取り → `.decode(errors='replace')`
- [ ] `ps aux` 検出プロセスも `log_size > 0` で completed 判定
- [ ] monitor iter ごとに `raw/monitoring/YYYY-MM-DD.md` 詳細 + `log.md` 1行サマリ
- [ ] 全 `running=0` で `CronDelete` 自殺

### 落とし穴

- [ ] `--bare` は OAuth を読まない (mistakes A-8) → Max 20x 完結なら禁止
- [ ] CronCreate の cron は :00/:30 を避ける (`2-59/5 * * * *` のようなオフセット)
- [ ] `claude --print` 引数渡しは 1MB まで、超える prompt は別ファイル経由
- [ ] 子セッションは親シェルの cwd を継承、必要なら `launch_session.sh` 内で `cd`
- [ ] 完全バイパス (`--dangerously-skip-permissions`) でも子に「git push / rm -rf / sudo 禁止」をシステムプロンプトで入れる (YD 明示指示時は省略可)

---

## 📚 関連

- [[2026-05-20_parallel-claude_監視基盤構築]] — 構築の意思決定記録
- [[claude_code]] — Claude Code 全般ノウハウ
- [[claude_code_permissions]] — 権限モード詳細
- [[2026-05-19_API依存撤廃_Max20x完結化]] — 「Max 20x 枠完結」原則
- [[claude_mistakes]] A-11 / A-12 — 今回発見した新ミスパターン

## 🌐 外部参考

- Anthropic 公式: `claude --print --output-format stream-json` のドキュメント (`claude --help`)
- `~/projects/business-plan-sprint-2026-05-19/launch.sh` — 同系統の別実装、`_pids.txt` ベース

## 📝 このノートの更新タイミング

- 監視 prompt の改善案を発見した時
- 別系統スクリプトとの統合パターンが増えた時
- Claude Code の重大な仕様変更があった時
