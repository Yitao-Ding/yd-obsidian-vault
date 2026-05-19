---
type: current_state
last_updated: 2026-05-20 03:20
ephemeral: true
expires: 2026-05-20 翌朝確認したら削除
---

# 朝の引き継ぎメモ (2026-05-20 深夜→朝)

> このファイルは「YD が朝起きてやることリスト」。一時的なメモ。
> 確認したら削除して OK (current_state の整合性のため)。

---

## ✅ 今夜 (2026-05-20 03:00〜03:20) Claude Code がやったこと

### 1. 機能マッピング自動化 (B案、フル実装)

- `current_state/available_capabilities.md` 新規 — スキル 40+ / MCP 11 / トリガー語彙 / プロジェクト別機能候補
- `00_CLAUDE_BOOT.md` に Step 5「機能マッピング演習」挿入 (旧 5→6, 6→7)
- `~/.claude/CLAUDE.md` 必須読み込みに #8 として追加 + 演習セクション追記
- `decisions/2026-05-20_機能マッピング自動化.md` 新規 (必須3セクション付き)
- `log.md` 1行追記

→ **次の新セッションから「おはよう」一発で自動マッピング演習が走る**

### 2. morning-briefing への反映

- `synthesizer/briefing.py` — `capabilities` フィールド + Vault context 読み込み + プロンプト/スキーマ更新 + TTS反映
- `templates/briefing.html.j2` — セクション 06「今日使えそうな機能」追加、旧 06→07
- `templates/briefing.css` — `.card-cat.capability` (teal) 追加

### 3. GitHub Private リポジトリ作成 + push

- **morning-briefing**: `https://github.com/Yitao-Ding/morning-briefing` (新規 Private)
- **Vault**: `aa3b3b4 → cd9484e` push 済

### 4. cron 登録

- `30 7 * * *` で `run.sh` を毎朝 07:30 JST 実行
- ログ: `~/projects/morning-briefing/logs/cron.log`
- **初回 (明朝) は Google Drive 認証がまだなので upload 部分が失敗します**
- PDF と MP3 はローカルに生成される (`~/projects/morning-briefing/output/2026-05-20/`)

---

## 🔴 YD が朝起きてやること (15分程度)

### Step 1: 朝の cron 実行ログを確認

```bash
tail -50 ~/projects/morning-briefing/logs/cron.log
```

期待される内容:
- `[YYYY-MM-DD HH:MM:SS JST] morning-briefing start`
- `[1/4] Collecting raw data ...` `collected: news=N podcasts=N ...`
- `[2/4] Synthesizing via claude -p ...`
- `[3/4] Rendering ...`
- `[4/4] Upload ...` ← **ここでエラーになるはず** (Drive 認証未完了)
- ローカル成果物: `~/projects/morning-briefing/output/2026-05-20/` に PDF + MP3

### Step 2: Google Drive OAuth 認証 (ブラウザ操作必須)

これは Claude Code 側で代行不可。YD の手作業:

```bash
# 1. Google Cloud Console で OAuth クライアントを発行 (まだの場合)
#    https://console.cloud.google.com/apis/credentials
#    → 「OAuth クライアント ID」→「デスクトップアプリ」で作成
#    → ダウンロードした JSON を以下に配置:
#    ~/projects/morning-briefing/credentials/client_secret.json

# 2. ブラウザ認証
cd ~/projects/morning-briefing
uv run python -m src.uploader.drive --auth
# → ブラウザが開く → Google アカウントで承認 → token が credentials/token.json に保存

# 3. 認証が通ったら、手動でフル実行 (今朝の分を Drive に上げる)
./run.sh
```

完了後の確認:
```bash
ls -la ~/projects/morning-briefing/credentials/
# → client_secret.json + token.json (or .pickle) があれば OK
```

### Step 3: 翌朝以降は完全自動

Step 2 が一度通れば、以降は毎朝 07:30 に cron が自動配信 (Drive `Morning Briefing/2026-05/` フォルダに PDF + MP3 が積まれる)。

### Step 4: このファイルを削除

```bash
rm ~/ObsidianVault/current_state/handover_morning.md
```

---

## 🟡 寝てる間に進められなかったもの (YDの認証/手作業必須)

| 項目 | なぜ自動化できなかったか | 必要な YD 作業 |
|------|----------------------|--------------|
| morning-briefing Drive 認証 | OAuth はブラウザ操作必須 | 上記 Step 2 |
| vidkit lecture モード仕上げ | HuggingFace で `pyannote/speaker-diarization-community-1` の利用申請が必要 | HF アカウントで申請 → `.env` に `HF_TOKEN` 設定 |
| Salamat WBS Phase 3 GitHub push | `gh repo create salamat-website-v2` を YD アカウントでやるか不確定だった | YD 判断 (private/public、organization 配下にするか) |

→ いずれも安全のため触らず、確認事項として残しました。

---

## 🟢 寝てる間に進んでいるかもしれないもの

- **ai-researcher** (launchd 経由、毎時 HH:03 で collect 走行) — 朝 raw/research/2026-05-20/ に記事が積まれているはず
- **明朝 07:30** の cron 実行 (上記の通り、Drive アップロード以外は完走するはず)

朝のログ確認コマンドまとめ:
```bash
# morning-briefing cron
tail -80 ~/projects/morning-briefing/logs/cron.log

# ai-researcher
ls -la ~/ObsidianVault/raw/research/2026-05-20/
uv run --directory ~/projects/ai-researcher ai-researcher status
```

---

## 📝 関連

- [[2026-05-20_機能マッピング自動化]] — 今夜の意思決定
- [[available_capabilities]] — 新規マッピング表
- [[morning_briefing]] — パイプライン仕様 (capabilities 反映済を反映する形で要更新)
- [[active_projects]] — morning-briefing 状態を「cron 登録済、Drive 認証待ち」に更新する候補
