---
type: decision
date: 2026-05-19
recorded: 2026-05-20
category: ops / git
project: Task Hub
related:
  - "[[active_projects]] #6 Task Hub"
  - "[[task_hub]]"
  - "[[vidkit]]"
---

# Task Hub の git 整理 + GitHub Private 連携 (8週前 Initial commit を 5 commit に統合)

## 背景

- 2026-03-24 の `Initial commit from Create Next App` 直後から実装を継続したが、その後 **8 週間ノーコミット**で放置
- 結果: untracked 32 ファイル + dirty 7 ファイル (`src/app/(app)/` / `src/components/` / `src/lib/` / `src/types/` / `src/contexts/` / `public/icons/` / `scripts/` + `HANDOVER.md` / `salamat-task-hub-spec.md` / Firebase 設定 4 種)
- GitHub remote 未設定、デプロイは Firebase Hosting (`salamat-task-hub.web.app`) で稼働中
- 監視 CC が `.gitignore` に `serviceAccountkey.json` 多重保護を 2026-05-19 21:00 頃に追記済 → 流出リスクは消失、ただしコミット粒度は壊滅的のまま
- YD が「Task Hub の git を整理して」と明示指示 (2026-05-19 21:14 頃)

## 選択肢

- A. 1 commit に一括統合 (シンプルだが diff が壊滅的、レビュー不可)
- B. **3〜5 コミットに論理分割** (依存順序を考慮)
- C. もっと細分化 (10+ コミット、git history が綺麗だが overkill)

## 決定

**B** (5 コミットに分割) を採用。

## 理由

- 依存順序が綺麗に並ぶ: build config → Firebase 設定 → app core → PWA → docs
- diff のレビュアビリティ確保 (将来 YD 本人 / 別 CC が振り返るとき)
- future revert を 1 commit 単位で安全に
- vidkit の git 初期化 (2026-05-19、3 commit) と同じ思想を踏襲

## 実行内容 (5 commit、依存順)

| # | hash | type | 主な変更 |
|---|---|---|---|
| 1 | `f876634` | chore | Next.js 静的 export 設定 + `firebase` / `firebase-admin` / `next-pwa` 依存追加 + `.gitignore` に `.firebase/` 追加 |
| 2 | `6d0e341` | feat | `.firebaserc` / `firebase.json` / `firestore.rules` / `storage.rules` |
| 3 | `7fa1b65` | feat | `src/types` / `src/lib` / `src/contexts` / `src/components` / `src/app/(app)` / `src/app/login` + `layout`/`page`/`globals` 更新 |
| 4 | `47b6be5` | feat | `public/manifest.json` + `sw.js` + icons (5枚) |
| 5 | `9dfcd77` | docs | `HANDOVER.md` + `salamat-task-hub-spec.md` + `scripts/set-admin.mjs` |

GitHub Private リポジトリ作成 + push (1 コマンド):
```bash
gh repo create Yitao-Ding/salamat-task-hub --private --source=. --remote=origin --push
```

結果: https://github.com/Yitao-Ding/salamat-task-hub (Private、`main → origin/main` 追跡済)。

## 副次的な発見

- `active_projects` #6 の Task Hub 記述で長らく「✅ Vercelデプロイ完了」と書かれていたが、実態は **Firebase Hosting** (HANDOVER.md と firebase.json で確証)。長期間の誤記を訂正
- `knowledge/programming/tools/task_hub.md` が未作成だった (vidkit / lecture_hub などは存在) → 本作業に合わせて新規作成 ([[task_hub]])

## ✅ うまく行ったこと

- 5 コミット分割の依存順序がシンプル (1 → 2 → 3 → 4 → 5、相互依存なし)
- `serviceAccountkey.json` は監視 CC が事前に保護してくれていたので、`git add` の前に `git check-ignore -v` で確認するだけで済んだ
- `gh repo create --source=. --remote=origin --push` 一発で 「repo 作成 → remote 設定 → push」を完了
- 各 commit メッセージは日本語説明 + Conventional Commits prefix で統一 (vidkit と同スタイル)
- 監視 CC が「Task Hub 24 件 + 8 weeks ago N tick 連続不変」を検知し続けてくれていたので、本作業中の進捗が他 CC から可視化されていた

## ❌ 詰まったこと

- `src/app/(app)/` のディレクトリ名に括弧 `()` が入っているため、Bash で `git add` する際にシェルが特殊文字として解釈する可能性があった → シングルクォート `'src/app/(app)'` で囲んで回避
- `.firebase/hosting.b3V0.cache` が untracked になっていたが、これは Firebase Hosting のローカルキャッシュなので commit すべきでない → `.gitignore` に `.firebase/` を追加して除外
- `/out/` は既に `.gitignore` 行 18 で除外済だったが、見落とすところだった (静的 export 出力先と HANDOVER の `firebase deploy --only hosting` で読まれる先が一致)
- 1 度 `active_projects.md` の Edit が「File has been modified since read」で失敗 (監視 CC が同時編集していたため)。再 Read → 再 Edit で解消

## 📋 次回同じことをするときのチェックリスト

「Initial commit 止まり + 全実装が untracked」状態のリポジトリを整理する標準手順:

1. **状況把握** (Read-only、並列):
   - `git status --short` で modified / untracked を一覧
   - `git log --oneline --all` で過去履歴を確認 (Initial commit のみか、ブランチ複数あるか)
   - `git remote -v` で remote 設定の有無
   - `cat .gitignore` で除外設定を確認
   - `git check-ignore -v <疑わしいファイル>` で機密ファイルが守られているか確証
   - `gh auth status` で GitHub 認証状態と scope (`repo` 必須) 確認

2. **機密漏洩の事前確認** (最優先):
   - `serviceAccount*.json` / `.env*` / `*.key` などが untracked / modified に含まれていないか
   - 既に git 履歴に commit されていないか (`git log --all --full-history -- <file>`)
   - 含まれていれば、別タスクとして `git filter-repo` などで完全削除を検討

3. **コミット計画** (3〜5 個に分割):
   - 依存順序: 設定 (build/deps) → 基盤 (DB/auth config) → コア実装 → 補助機能 (PWA/分析等) → ドキュメント
   - 各 commit は「単独で意味が通じる」「単独で revert 可能」な粒度
   - `chore:` / `feat:` / `fix:` / `docs:` の prefix で揃える

4. **コミット実行**:
   - `git add` は **明示的にパス指定** (`git add .` は使わない、機密混入防止)
   - 特殊文字を含むパス (`(`, `)`, `[`, `]`) はシングルクォートで囲む
   - メッセージは HEREDOC で渡す (改行・引用符の安全性)
   - `Co-Authored-By` を付ける (vidkit と同スタイル)

5. **GitHub Private 連携** (1 コマンド):
   ```bash
   gh repo create <user>/<repo> --private --source=. --remote=origin --push
   ```

6. **Vault 反映**:
   - `current_state/active_projects.md` の該当エントリを更新 (commit hash / GitHub URL / 実態整合)
   - `decisions/YYYY-MM-DD_<内容>.md` に意思決定記録 (本ファイル)
   - `knowledge/programming/tools/<project>.md` に運用マニュアル (未作成なら新規)
   - `log.md` に 1 行追記
   - 関連する古い記述 (例: 「Vercelデプロイ」→ 「Firebase Hosting」) があれば同時訂正

7. **再発防止**:
   - 新規プロジェクト開始時、Create Next App 直後 = **Day 1 で必ず GitHub 連携**
   - 機密ファイル (`serviceAccountKey.json` 等) は **プロジェクト作成と同日に `.gitignore` に追加**
   - 監視 CC の「N weeks ago M tick 連続不変」検知に依存しすぎない (検知 → 動かないと意味がない)

## 関連

- [[active_projects]] #6 Task Hub
- [[task_hub]] (新規作成、2026-05-20)
- [[vidkit]] — 類似の git 初期化パターン (2026-05-19 完了、Yitao-Ding GitHub 個人 account)
- 監視 CC の log.md tick #1〜#9 — 「Task Hub 24 件 + 8 weeks ago N tick 連続不変」と検知してた経緯
