---
date: 2026-05-19
type: decision
category: technical
tags: [vidkit, fcpxml, tighten, tutorial, vault-integration, claude-code]
---

# vidkit に tighten / tutorial / --vault-path を一気に実装

## 背景

vidkit は autocut までで止まっていて、進行中プロジェクトに次の作業が並んでいた:

1. tighten オペレーション (FCPXMLラウンドトリップ第一弾)
2. FCPXMLリーダーの汎用モジュール化
3. tutorial モード (Webサイト制作チュートリアル動画 → 実装まで一気通貫)
4. Vault連携 (`--vault-path` オプション)
5. lecture モード仕上げ (HF_TOKEN 取得待ち)

YD から「未完成の機能を全部、聞かなくて良いから勝手にどんどん進めて」との指示を受けて、
HF_TOKEN 待ちの lecture 以外をワンセッションで完走させた。

## 決定

| 項目 | 決定内容 |
|------|---------|
| **FCPXML リーダーの置き場** | `fcpxml.py` に追記 (新規ファイルは作らない) — 既存のヘルパー (`_pick_frame_duration`, `_snap_to_frame`, `_rational`, `_file_url`, `_audio_rate_attr`) を共用するため |
| **時刻基底の統一** | パーサ側で全ての numerator を sequence の `frameDuration` denominator (`fd_den`) に rebase。下流コードは単一の den で扱える |
| **tighten の位置** | `vidkit/tighten.py` 新規。fcpxml (シリアライズ) と silence (検出) の合成として独立させる |
| **tutorial パイプライン** | dance パイプラインに相乗り (Whisper + シーン検出 + フレーム抽出)。差分は `prompts/tutorial.md` のみ |
| **Vault 出力パス** | `--vault-path <root>` 指定時、`<root>/raw/vidkit/<mode>/vidkit_<date>_<mode>_<id>/` に出力。指定なしは従来の `~/Downloads/` 維持 |

## 実装ハイライト

### `parse_fcpxml(path) -> ParsedFCPXML`

- `<asset-clip>` を `ClipRef` (name / ref / offset_num / start_num / duration_num / extra_attrs) に変換
- `extra_attrs` に管理外属性 (audioRole / lane / format / …) を退避して round-trip 時に carry-over
- `<media-rep src="file://...">` を `Path` に逆変換 (`urlparse + unquote`)
- 構造的に欠けている (resources / library / event / project / sequence / spine) と即 `ValueError`、無理に補完しない
- MVP は単一 sequence + フラット spine 想定。compound / multicam / titles / transitions は無視

### `tighten(parsed, preset_name, …) -> (new_parsed, stats)`

- 各 ClipRef について、media-rep が指す動画ファイルから `[start_sec, start_sec+dur_sec)` の窓を 16kHz mono WAV で抽出 (ffmpeg `-ss <s> -i <src> -t <d>`)
- その WAV に `detect_silence` → `silence_to_keeps` (autocut と同じ preset)
- 得られた keeps (クリップ内相対秒) を `clip_start + k.start/k.end` で source-time に戻し、`snap_to_frame` で frame-numerator 化
- 全クリップ走査後に offset を 0 から累積和で再計算
- 元の Asset が見つからない / src ファイルが消えている クリップは「pass-through」(警告だけ出して原状維持)。dropしない (副作用を最小化)

### CLI / pipeline 統合

- `VALID_MODES` に `tighten` と `tutorial` を追加
- `run()` の冒頭分岐に `tighten → _run_tighten` を追記
- `_resolve_out_root(args)` を切り出して `--vault-path` の優先ルールを 1 ヶ所に集約

## 検証

合成 12 秒テスト動画 (3 発話 + 2 無音、30fps) で 2 パターン:

**(A) autocut → tighten のラウンドトリップ**
- autocut: 1動画 → 3 keep segments (8.4s 残し、3.6s 削除) → autocut.fcpxml
- tighten on autocut.fcpxml: 3 clips → 3 clips、追加削除 0.0s (期待通り、既に詰まり切ってる)

**(B) ラフ FCPXML (1 clip = 動画全体) に tighten 単独**
- 入力: クリップ 1 個 (offset=0s, start=0s, duration=36000/3000s = 12s)
- 出力: 3 clips
  - offset=0s, start=0s, duration=6300/3000s (元動画 0–2.1s)
  - offset=6300/3000s, start=11700/3000s, duration=9600/3000s (元動画 3.9–7.1s)
  - offset=15900/3000s, start=26700/3000s, duration=9300/3000s (元動画 8.9–12.0s)
- 削除合計: 3.6s ✓
- `xmllint --noout` 通過 ✓
- offset 累積和: 0 → 6300 → 15900 (= 6300 + 9600) ✓
- フレーム整列: 全 numerator が 100 の倍数 ✓

**(C) tutorial モード (`--whisper skip`)**
- dance パイプラインに乗り、prompts/tutorial.md がプレースホルダ展開込みで PROMPT.md として出力された
- 出力ディレクトリ構造は dance と同じ (meta.json / scenes.md / transcript.md / frames/ / PROMPT.md)

**(D) `--vault-path ~/ObsidianVault` 指定**
- 出力先が `~/ObsidianVault/raw/vidkit/tighten/vidkit_2026-05-19_tighten_rough/` に切り替わることを確認 (検証後にテスト出力は削除)

**(E) 実機 47分講義動画でのラウンドトリップ** (追加検証、2026-05-19 深夜)
- 入力: `~/Downloads/オンデマンド経営学５-3.mp4` (852x480 / variable-fps avg 30.302 / 47m31s)
- **発見した既存バグ**: ffprobe が `r_frame_rate=600/1` (Zoom録画系のクロック単位) を返すため `fps=600.0` と誤検出 → `_pick_frame_duration` が即 raise
- **fix** (commit 40cef7d):
  - `fetch.py` に `_pick_fps(r, avg)` を追加。`r > 120 かつ |r - avg| > 1.0` で `avg_frame_rate` を採用
  - `fcpxml.py` の `_pick_frame_duration` 許容を 0.1 → 0.5 に緩めて 30.302 → 30 にスナップ可能化
- **結果**:
  - autocut: 479 keeps、削除 196.6s (2.25s で完走)
  - autocut→tighten: 479 → 483 clips、追加削除 0.8s (ほぼ冪等、autocut のリッパに残った微小無音を拾った)
  - rough(1clip)→tighten: 1 → 479 clips、削除 196.9s (autocut と整合、誤差 0.3s は端数)
  - xmllint 通過、フレーム整列OK
- これで Zoom 録画 / オンライン講義 / screen capture 系全般が autocut/tighten に通る

## 配布・運用 (2026-05-19 完了分)

- **GitHub Private repo 化**: https://github.com/Yitao-Ding/vidkit
  - Initial commit (実装一式) + 2026-05-19 docs commit + 2026-05-19 variable-fps fix commit
- **Skill 登録** (新セッションでキーワード自動ロード):
  - `~/.claude/skills/fcp-tighten/SKILL.md` — クリップ内無音 / FCPXMLラウンドトリップ
  - `~/.claude/skills/video-tutorial/SKILL.md` — チュートリアル動画 / follow along / 動画から実装
- **ユーザーガイド** (`docs/`):
  - `docs/lecture-setup.md` — HF_TOKEN 取得 5 ステップ
  - `docs/tighten-howto.md` — FCP Export XML → tighten → Import XML 往復手順

## 残課題

- **lecture モード**: `docs/lecture-setup.md` で手順整備済。YD が HuggingFace で `pyannote/speaker-diarization-community-1` の利用条件同意 + Read token 発行 + `.env` への記入 (5–10 分)
- **実FCPプロジェクトでの tighten 動作確認**: 合成 FCPXML と実講義動画でのラウンドトリップは通過。FCP が手動エクスポートした XML (audioRole / lane / 複数 sequence) はまだ未検証 — YD のFCPプロジェクトが揃ったタイミングで `docs/tighten-howto.md` 参照
- **OBJ-C class 重複警告**: `cv2` と `av` の libavdevice が衝突して起動時に warning が出る (動作には影響なし)。autocut セッションから持ち越しの既知問題

## 関連

- [[2026-05-18_FCPXML_ラウンドトリップ採用]] — 設計判断の起点
- [[vidkit]] (knowledge/programming/tools/) — 機能一覧
- `~/projects/vidkit/vidkit/fcpxml.py`, `tighten.py`, `prompts/tutorial.md`

---

## ✅ うまく行ったこと

- **既存ヘルパーの共用**: `_snap_to_frame` を `snap_to_frame` として別名公開するだけで tighten 側から再利用できた。autocut のフレーム整列ロジックを 1 行も書き直さずに済んだ
- **denominator 統一の事前変換**: パーサが per-element den を sequence den に rebase したおかげで、tighten 側のオフセット計算が「整数の足し算だけ」に収まった。float に落とすバグの余地が消えた
- **ClipRef.extra_attrs で属性 carry-over**: 管理外属性をそのまま辞書に退避する設計で、未来の FCPXML 拡張属性 (lane / audioRole / format / …) に手を入れずに耐性が出た
- **tutorial を dance に相乗り**: 「処理は素材準備のみ、思考は Claude Code 側に任せる」設計思想と整合。新パイプラインを書かずに済んだ
- **`--vault-path` を 1 ヶ所集約**: `_resolve_out_root(args)` で分岐を一極化、各モードは何も知らずに恩恵を受ける
- **YD の "全部勝手にやって" の許可**: 確認質問なしで T2 → T8 を直列に通せたので、所要時間が読みやすかった
- **実機 47分動画での tighten 冪等性**: autocut → tighten で +0.8s しか追加削除されなかった (= 0.03%)。tighten ロジックが「autocut で詰めた状態」を破壊せず、純粋に取りこぼし無音だけ拾えていることが実証された
- **autocut の処理速度**: 47.5分動画を 2.25 秒で完走 (CPU 235%、ffmpeg silencedetect は I/O 律速)。「素材準備は秒で終わる、考えるのは Claude Code 側」の設計思想と整合
- **variable-fps 発見が rough→tighten で実りに**: バグ fix の副産物として、Zoom 録画 / オンライン講義 / screen capture 系全般 (= Salamat の議事録録画系も含む) が自動的に autocut/tighten に通るようになった
- **GitHub Private push が gh CLI 1 コマンド**: `gh repo create vidkit --private --source=. --remote=origin --push` で repo 作成→push→origin 設定が一発。リポジトリ作成スクリプト不要

## ❌ 詰まったこと

- **ffmpeg silencedetect 時の audioChannels = 0**: aevalsrc 由来のテスト音声で `audioChannels="1"` が出た (元動画の channels が ffprobe で読めないケース)。実害なし (FCP は読める) だが、より堅牢にするなら ffprobe fallback を増やす余地あり
- **テスト動画の sine + 完全無音設計**: 「無音区間で `silencedetect` がきれいに反応する」状態を作るために aevalsrc の `between(t, a, b)` を覚え直す必要があった。一発で通せず、最初 `aevalsrc='sin(2*PI*440*t)*if(lt(t,2)+gt(t,4)*…'` で構文エラー
- **--vault-path 動作確認時にテスト出力が Vault 内に残る**: 動作OK確認後に手動 rm が必要だった。今後は `--out /tmp/vidkit_test` のような明示的テスト先で検証すべき (vault-path はあくまで本番経路の確認だけにする)
- **OBJ-C class 重複警告**: 起動時に必ず stderr に出る。autocut 時から放置していた既知問題で、cv2 と av が同じ libavdevice を別バージョンで持つことが原因。実害ないが、一度 `uv pip uninstall av` か `--no-deps` で再 install するか要検討
- **variable-fps 動画で `r_frame_rate=600/1`**: ffprobe が Zoom 録画系の MP4 に対して r_frame_rate を「サンプリングクロックの最大値 (600/1)」として返す仕様に最初気付かなかった。実 fps は `avg_frame_rate=8641100/285163 ≈ 30.30`。fetch.py で r_fps 妄信していたので即 `_pick_frame_duration` で死亡。fix 後は r_fps が 120 超 かつ avg と乖離する場合だけ avg にフォールバック (固定 fps 動画では従来挙動を維持)
- **`_pick_frame_duration` の許容 0.1 が厳しすぎ**: 30 と 29.97 を区別したい意図で 0.1 にしていたが、screen recording の 30.302 すら弾いてしまっていた。0.5 まで緩めても 30/29.97/25/24 は ≥1.0 離れているので区別可能。許容範囲は「区別したい最近接ペアの距離」で決めるべきだった
- **長い日本語パスのシェル escape**: `~/Downloads/オンデマンド経営学５-3.mp4` のような日本語+全角数字パスでも `"..."` で囲めば動く。一度 quote 忘れて `ZSH: no matches found` 系のエラーが出た

## 📋 次回同じことをするときのチェックリスト

### FCPXML ラウンドトリップで新オペレーションを追加するとき

- [ ] `parse_fcpxml` の戻り値 `ParsedFCPXML` を出発点にする (新パーサを書かない)
- [ ] **時刻計算は全部 numerator (整数) ベース**。秒に落とすのは ffmpeg に渡す瞬間と人間向け表示だけ
- [ ] 既存 `snap_to_frame` を使って source-time を frame 整列させる
- [ ] 新オペレーションは `vidkit/<op>.py` に独立モジュールとして置く
- [ ] CLI 統合は `VALID_MODES` 追加 + `pipeline.run()` の分岐 + `_run_<op>` 関数の 3 点だけで済むよう設計
- [ ] 合成 FCPXML (`write_fcpxml` で 1 clip を直書き) + autocut 出力の 2 パターンで検証
- [ ] `xmllint --noout` 必ず通す
- [ ] 「未知 asset / 行方不明 src 」のフォールバックを最低限決めておく (pass-through or drop)

### 新モードを `dance` 系パイプラインに相乗りさせるとき

- [ ] `VALID_MODES` に追加
- [ ] `prompts/<mode>.md` を新規作成 (既存 dance.md と同じプレースホルダ語彙: `{title}` `{duration}` `{scene_count}` `{frame_count}` `{avg_cut_length}` `{speaker_count}`)
- [ ] `_template_for(args)` に分岐追加 (`dance` 分岐の手前か直後)
- [ ] 話者分離不要なら `_diarize_is_active` 既定で OFF になることを確認 (`auto` は lecture のみ)
- [ ] `--whisper skip` で動作確認 → モデル DL 不要で素早く E2E

### `--vault-path` 経路で出力を確かめるとき

- [ ] **テスト出力は `--out /tmp/...` に向ける**。`--vault-path` は本番経路の最終確認だけにする
- [ ] Vault に出力したものは raw/ 配下なので、不要なら即 `rm -rf` してOK (raw は再生成可能なデータの置き場)
- [ ] 出力後に Vault git status で混入確認 (`vstatus`)

### variable-fps / 非標準 fps の動画を vidkit に通すとき

- [ ] **ffprobe で `r_frame_rate` と `avg_frame_rate` の両方を見る**。Zoom録画 / screen capture / browser MediaRecorder は r が 600/1 のような擬似値を返す
- [ ] `r > 120 かつ avg と 1.0 以上乖離` していたら **avg を信用**する (commit 40cef7d で実装済)
- [ ] スナップ許容は **0.5 fps**。30.302 → 30、29.83 → 29.97 のように寄せる。FCP に 30 として渡しても映像/音声タイミングは劇的にはずれない
- [ ] それでも snap できない fps (15 / 100 / 任意 VFR) は **事前 transcode** が必要: `ffmpeg -i input -c:v libx264 -r 30 -c:a copy output.mp4`

### vidkit を GitHub Private repo 化するとき (再現用)

- [ ] `.gitignore` に `.venv/` `.env` `__pycache__/` `uv.lock` `/tmp/vidkit_*/` が入っていることを確認
- [ ] `git init -b main` → `git add -A` → status で .env / .venv / __pycache__ が staged にないことを確認
- [ ] 初回 commit (本文に Co-Authored-By: Claude Opus 4.7 を入れる)
- [ ] `gh repo create <name> --private --source=. --remote=origin --push --description "..."` の 1 行で repo 作成 + push + origin 設定が完了する (gh CLI 必須、要 `repo` scope の token)

### 新規 Skill (`~/.claude/skills/<name>/SKILL.md`) を作るとき

- [ ] frontmatter の `description` に **トリガーキーワードを多言語 (日/英)** で列挙する。Claude Code は description で skill を選ぶので、ここが薄いと拾われない
- [ ] **Do NOT use for: ...** で姉妹 Skill との棲み分けを明示する (例: fcp-tighten から fcp-autocut への振り分け)
- [ ] 新セッションで `tool_search` でロードされるので、現セッションでは即時には反映されない。テストは新セッションで
