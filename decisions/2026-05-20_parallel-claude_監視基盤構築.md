---
type: decision
date: 2026-05-20
domain: parallel-claude, monitoring, claude-code
status: implemented
tags: [claude-code, parallel, nohup, cron, monitoring, max20x]
---

# 2026-05-20: parallel-claude 監視基盤の構築 (寝てる間の自走システム)

## 状況

深夜 03:15 頃、YD が「これから色んな Claude Code を走らせてくれ、全部監視してくれ、Obsidian に監視方法が書いてある、寝るから自走で、承認いらない、全部権限バイパスで」と依頼。さらに「ターミナル閉じても Mac が自動的にやってくれるプロンプトも別に打ったから、それも込みで監視して」(= `business-plan-sprint-2026-05-19/launch.sh` で 12 並列が既に 03:13:44 に起動済) を後追い指示。

教科書 `textbook/03_ai_engineering/01_claude_code_parallel.md` に「セッションD = 監視ループ」のコンセプトは書かれていたが、**実体のスクリプトはまだ存在しなかった**。一から作る前提。

## 選択肢と決定

| 選択肢 | メリット | 不採用理由 |
|--------|---------|----------|
| A. `claude --print` + `nohup` + 親セッションが `CronCreate` で 5分ループ | OAuth (Max 20x) 完結で API 課金なし。stdout を JSONL ログに保存して後追い可能。新規プロセスは `ps aux` で自動検出可能 | 採用 |
| B. `tmux` で対話セッションを保持 | セッション ID が確定、resume 可能 | `--print` で十分、対話モード不要 |
| C. `launchd` でスクリプト化 | macOS 標準、durable | 過剰、cron-create は session-only でも 7日持つ |

**決定: A**。理由:
- YD の Max 20x 枠で完結 ($0)、API 課金なし
- 親セッション 1 つ + 子 N セッションの単純構造
- 監視は別プロセスではなく親 Claude Code がループで叩く形 (`CronCreate` で `2-59/5 * * * *`)
- `ps aux | grep 'claude.*(-p|--print)'` で新規プロセスを自動検出 → 「YD が後で起こすかもしれない他のセッション」もカバー

## 実装

### 構成

```
~/projects/parallel-claude/
├── README.md
├── state/sessions.json     全セッション状態 (PID, status, log_file, ...)
├── tasks/                  各セッションの prompt (5本)
├── logs/                   stream-json 出力
└── scripts/
    ├── launch_session.sh   1セッション起動
    ├── launch_all.sh       5並列を一気に起動
    ├── monitor.sh          状態スキャン + 外部 _pids.txt 取り込み + ps検出
    └── status_report.sh    整形出力
```

### 起動コマンド

```bash
nohup claude \
  --print \
  --dangerously-skip-permissions \
  --output-format stream-json --verbose \
  --model opus \
  --append-system-prompt "..." \
  "$PROMPT" \
  > "$LOG_FILE" 2>&1 &
```

### 監視ループ

`CronCreate(cron="2-59/5 * * * *", recurring=true)` で 5 分ごと発火。`*/5`(:00 集中) を避けるため :02/:07/:12... に振った。

### 結果 (1時間後、iter=12 で全終息)

| メトリック | 値 |
|----------|-----|
| 監視イテレーション | 12 |
| parallel-claude 完了 | 5/5 |
| BPS 終了 (旧+新 24本中) | 24 (うち running 0) |
| BPS candidates | 50本 |
| BPS critiques | 61本 |
| FINAL_REPORT.md | 29KB (LegalTrio / 越境EC / 税理士SaaS の3案) |
| Red Team `_meta_review.md` | 11KB (「規制空白=チャンス」誤読を15-20候補に検出) |
| 実支払い | **$0** (Max 20x 完結) |

---

## ✅ うまく行ったこと

- **Max 20x 完結化**: `--dangerously-skip-permissions` + OAuth で `apiKeySource:"none"` 確認、API 課金 $0 (スモークテストの cost_usd 表示は Sonnet 換算の参考値)。
- **外部 PID ファイル import**: business-plan-sprint が独自に `_pids.txt` を吐くため、それを monitor.sh で読んで自分の `state/sessions.json` に統合。**二重起動の発見と回避**もこのおかげ (YDの指示で launch.sh を叩く前に `_pids.txt` の中身を読んで「既に動いてた」と気付いた)。
- **`ps aux` 検出**: `claude -p` も `claude --print` も拾える正規表現 `(\s|^)(-p|--print)(\s|$)` で、別系統のセッションも未登録なら自動取り込み。
- **CronCreate の :02/:07 オフセット**: fleet 全体への影響は限定的だが、tool description の指示通り :00 を避けた。
- **教科書の図解 (3.4 cron / 3.5 git で衝突回避) がそのまま運用設計に直結**: 「役割分担で衝突しないようにする」「共有ファイル (log.md) は追記専用」を実装に反映、衝突なし。
- **stream-json の verbose 出力**: 各セッションの内部状態 (system/init / assistant / tool_use / result) が全部 JSONL で残り、死亡時の原因解析が即座にできた。
- **CronCreate (session-only) が想定通り**: durable=false でも 7 日持つので、寝てる間の 4-5 時間は十分カバー。

## ❌ 詰まったこと

- **`subprocess.run(text=True)` の UTF-8 strict decode**: `tail -3` のマルチバイト境界切断で `UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb3 in position 0: invalid start byte` で iter=6 が死んだ。**修正**: `capture_output=True, text=False` で bytes 受けて `.decode('utf-8', errors='replace')` に書き換え。[[claude_mistakes]] A-11 として記録。
- **状態判定の片肺**: `_pids.txt` 取り込み時は `log_size > 0 && PID dead` を completed と判定するが、`ps aux` 経由で取り込んだ「discovered_<pid>」プロセスは PID 死亡で常に `died` 扱い (log にちゃんと完了結果が残ってても)。fail/died カウントが過剰に膨らんだ。[[claude_mistakes]] A-12 として記録。
- **BPS の二重起動が裏で発生**: 03:13:44 起動の旧12本が動いてた直後の 03:23:24 に launch.sh が **何者かによって再実行**された (YD が別ターミナルで叩いた可能性が最も高い)。介入指示なしのため放置したが、`_pids.txt` が上書きされて旧PIDが消えた → ps aux で旧PIDが見えるが pid_file からは追跡不能、という分離状態に。**気付き**: 別系統の launch.sh は touch しない設計は良いが、`_pids_history.txt` のような append-only もあるとよかった。
- **Red Team が ScheduleWakeup で自己 reschedule して死亡**: 10_synthesis が「ウェイクアップ: 最大3600秒後」とログ出力したが、`--print` プロセスは1ターンで終わるので wake-up は発火しない設計矛盾。これは BPS 側の設計問題で、私は触らない指示。

## 📋 次回同じことをするときのチェックリスト

### 起動前

- [ ] **既に動いてるプロセスを ps aux で確認** (`ps -axo pid,etime,command | grep 'claude.*-p'`) — 二重起動の兆候を最初に潰す
- [ ] 別系統のスクリプトがある場合、その `_pids.txt` や log ディレクトリの存在をチェック (今回 BPS の `_pids.txt` の存在で「既に動いてる」を発見できた)
- [ ] 各タスクの prompt に「YDは寝てる、承認は得られない、完了したら DONE.txt を書いて終了」を明示 (今回 BPS の 14_hatachi が確認質問待ちで死亡したのは、この明示がなかったから)
- [ ] `--dangerously-skip-permissions` でも、各タスクに「git push / rm -rf / sudo は禁止」のガードレールはシステムプロンプトで入れる (今回は YD の「完全バイパス」指示で省略した)
- [ ] `apiKeySource:"none"` がスモークテストで確認できることを最初に検証 (Max 20x 完結の証跡)

### 監視中

- [ ] `subprocess.run` でテキスト取得する箇所は全部 `capture_output=True, text=False` + `.decode(errors='replace')` の組み合わせを使う (UTF-8 strict は危険)
- [ ] `ps aux` 検出で取り込んだプロセスも `log_size > 0` チェックで completed 判定する (現状 monitor.sh は外部 _pids.txt 経由でしか効いてない)
- [ ] 監視iter ごとに `~/ObsidianVault/raw/monitoring/YYYY-MM-DD.md` に追記、`~/ObsidianVault/log.md` には1行サマリ — 朝の YD が一望できる構造
- [ ] 全 running=0 になったら CronDelete で自分自身を停止 (今回ちゃんと動いた、iter=12 で `985dcb02` 削除完了)

### 落とし穴

- [ ] CronCreate の cron は :00/:30 を避ける (`2-59/5` のようなオフセット)
- [ ] `--bare` は OAuth を読まないので Max 20x 完結なら使わない (mistakes A-8)
- [ ] `claude --print` 引数渡しは ARG_MAX 1MB まで OK、それを超えるなら `--system-prompt-file` のようなオプションを検討
- [ ] 子セッションの cwd は親シェルの cwd を継承するので、`launch_session.sh` 内で必要なら `cd` する
- [ ] `--no-session-persistence` を入れると `~/.claude/projects/` に session 履歴が残らない (今回は入れてない、後追い可能にした)

---

## 関連

- [[parallel_claude]] — 運用マニュアル (knowledge/programming/tools/)
- [[claude_code]] — Claude Code 全般のノウハウ
- [[claude_code_permissions]] — 権限モード詳細
- [[2026-05-19_API依存撤廃_Max20x完結化]] — 「Max 20x 枠完結」原則の出所
- [[claude_mistakes]] A-11 / A-12 — 今夜発見した新パターン
- `~/projects/parallel-claude/` — 実装
- `~/ObsidianVault/raw/monitoring/2026-05-20.md` — 12 iter 分の生ログ
