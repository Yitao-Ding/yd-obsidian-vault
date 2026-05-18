---
type: knowledge
domain: programming/tools
created: 2026-05-18
last_updated: 2026-05-19
tags: [vidkit, video, claude-code, ffmpeg, fcpxml, davinci, tutorial]
status: active
---

# vidkit — 動画前処理 CLI ツール

> 動画 → Claude Code が分析可能な素材に前処理するためのCLIツール
> YD専用、ローカル稼働、Claude Max プランの枠で運用 (API課金回避)

## 🎯 目的

YD用途の動画作業を、Claude Code が直接扱える形に変換する:

- ダンス映像 → シーン分割 + フレーム抽出 + メタデータ (`dance` モード) ✅
- 講義動画 → 字幕 + 話者分離 + コーネル式まとめ (`lecture` モード) ⏳ HF_TOKEN待ち
- 編集対象動画 → 無音カット済みFCPXML (`autocut` モード) ✅ 2026-05-18
- 既存FCPプロジェクト → クリップ内の残り無音を再カット (`tighten` モード) ✅ 2026-05-19
- チュートリアル動画 → Claude Code が自走実装できる素材セット (`tutorial` モード) ✅ 2026-05-19

## 📂 プロジェクト構成

**パス**: `/Users/ittou/projects/vidkit`
**GitHub**: <https://github.com/Yitao-Ding/vidkit> (Private、2026-05-19 push)

```
vidkit/
├── pyproject.toml          # uv パッケージ管理
├── README.md
├── install.sh
├── .env.example            # HF_TOKEN 等
└── vidkit/
    ├── cli.py              # Typer CLI エントリポイント
    ├── pipeline.py         # asyncio.gather 並列処理オーケストレーター
    ├── modules/
    │   ├── fetch.py        # yt-dlp で動画取得
    │   ├── audio.py        # ffmpeg で音声抽出
    │   ├── transcript.py   # MLX-Whisper (large-v3) で文字起こし
    │   ├── scenes.py       # PySceneDetect でシーン検出
    │   ├── diarize.py      # pyannote.audio (community-1) で話者分離
    │   └── merge.py        # 結果統合
    ├── silence.py          # ffmpeg silencedetect ラッパ (autocut/tighten 共用)
    ├── fcpxml.py           # FCPXML 1.13 シリアライザ + リーダー (autocut/tighten 共用) ★2026-05-19 リーダー追加
    ├── tighten.py          # tighten 本体ロジック ★2026-05-19 新規
    └── prompts/
        ├── dance.md
        ├── lecture_brief.md
        ├── lecture_standard.md
        ├── lecture_deep.md
        └── tutorial.md      # ★2026-05-19 新規
```

## 🛠 スタック

- **言語**: Python 3.x
- **パッケージ管理**: uv (Astral)
- **CLI**: Typer
- **並列処理**: asyncio.gather
- **動画取得**: yt-dlp
- **音声処理**: ffmpeg, ffprobe
- **文字起こし**: MLX-Whisper (large-v3 デフォルト)
- **話者分離**: pyannote.audio (community-1 モデル)
- **シーン検出**: PySceneDetect
- **無音検出**: ffmpeg silencedetect (autocut)
- **FCPXML生成**: 自作シリアライザ (autocut)

## 🎬 モード一覧

### 1. dance モード ✅完成

ダンスMV・パフォーマンス動画の分析向け。

**処理内容**:
- シーン検出メイン
- フレーム抽出 (1080p維持)
- 音声トラックも字幕化
- meta.json、scenes.md、frames/、PROMPT.md を出力

**実機テスト**:
- 2026-05-18: SaveClip.mp4 (9秒、tinyモデル)
- 2026-05-18: TIME_Instagram_最終２.mp4 (4K 60fps、15秒、large-v3)
  - 12カット検出、平均1.3s、最長3.8s、最短0.3s
  - 処理時間: 281秒 (モデルDL含む)

### 2. lecture モード ⏳未完成

講義・トーク動画の分析向け。

**処理内容 (予定)**:
- 字幕生成メイン
- 話者分離 (pyannote)
- コーネル式ノート補完用フォーマット

**残課題**:
- pyannote の HF_TOKEN セットアップ
- lecture_brief / standard / deep プロンプトの調整

### 3. autocut モード ✅完成 (2026-05-18 追加)

FCP用無音カット FCPXML 生成。

**処理内容**:
- ffmpeg silencedetect で無音区間を検出
- keep-segment を計算
- FCPXML 1.13 形式で出力 (フレーム精度の有理数時刻計算)

**プリセット**:

| プリセット | threshold | min silence | pad | 用途 |
|----------|-----------|-------------|-----|------|
| `lecture` | -30dB | 0.4s | 0.1s | 講義・トーク |
| `vlog` | -35dB | 0.8s | 0.2s | Vlog・インタビュー |

**プリセット上書き** (各フラグで個別指定可):
- `--silence-threshold` (例: -40)
- `--min-silence` (例: 0.6)
- `--pad`

**検証結果 (12秒テストクリップ、30fps、3発話+2無音)**:

| プリセット | keep数 | 残し時間 | 削除時間 |
|----------|-------|---------|---------|
| lecture | 3 | 6.4s | 5.6s |
| vlog | 3 | 6.8s | 5.2s |

**XML検証**:
- ✅ xmllint --noout 通過
- ✅ frameDuration 100/3000s (30fps NDF)
- ✅ 全 offset/start/duration がフレーム整列 (100の倍数 / 3000)
- ✅ offset が累積和 (0 → 6300 → 12900)
- ✅ media-rep src が絶対 file:// URL
- ✅ audioRate が文字列 44.1k (sequence) と整数 44100 (asset)

**Skill登録**: `~/.claude/skills/fcp-autocut/SKILL.md`
- FCP/FCPXML/無音カット等のキーワードで自動ロード

**使い方**:
```bash
# Claude Code から
「~/Movies/talk.mp4 をFCP用に無音カットして」

# 内部実行
uv run vidkit autocut <video> --preset lecture
```

**FCP起動**: 自動起動しない。パス表示のみ。ユーザーが「開いて」と言ったら:
```bash
open -a "Final Cut Pro" /path/to/autocut.fcpxml
```

### 4. tighten モード ✅完成 (2026-05-19 追加)

既存FCPプロジェクトの各クリップ内に残った無音をさらに詰める (FCPXML ラウンドトリップ第一弾)。

**入出力**:
- 入力: FCP で `File → Export XML` した `.fcpxml` ファイル
- 出力: 同じ構造で、各 asset-clip がより細かく分割された `.fcpxml`

**処理フロー**:
1. `parse_fcpxml(path)` で FCPXML を `ParsedFCPXML` に変換
2. 各 `ClipRef` について、media-rep が指す動画ファイルから対応する音声窓を 16kHz mono WAV で抽出 (ffmpeg `-ss <start> -i <src> -t <dur>`)
3. その WAV に `detect_silence` + `silence_to_keeps` (autocut と同じ preset)
4. 得られた keep を source-time に戻し、`snap_to_frame` で frame-numerator 化
5. 全クリップ走査後に `offset` を 0 から累積和で再計算
6. `write_fcpxml_from_parsed` で書き出し

**プリセット**: autocut と共通 (`lecture` / `vlog`)。`--silence-threshold` / `--min-silence` / `--pad` / `--merge-gap` で上書き可。

**フォールバック**:
- 未知の asset を参照する clip → 警告 + pass-through (drop しない)
- src ファイルが行方不明な clip → 警告 + pass-through

**検証 (2026-05-19、12秒テスト動画 / 3発話+2無音 / 30fps)**:

| 入力 | 出力 | 削除 | 備考 |
|------|------|------|------|
| autocut.fcpxml (3 clips、既に詰め済み) | 3 clips | 0.0s | 期待通り (追加で詰める箇所なし) |
| rough.fcpxml (1 clip、12s 全体) | 3 clips | 3.6s | 期待通り (autocut と同等の結果) |

xmllint 通過、offset 累積和・フレーム整列・media-rep 絶対パス保持を確認。

**使い方**:
```bash
# 1. FCP で素材を荒く並べる
# 2. File → Export XML (Cmd+E) → rough.fcpxml
# 3. Claude Code に依頼
uv run vidkit tighten rough.fcpxml --preset lecture
# 4. FCP に戻り File → Import XML で tighten.fcpxml を取り込む
```

### 5. tutorial モード ✅完成 (2026-05-19 追加)

Webサイト制作・ライブコーディング系チュートリアル動画 → Claude Code が「動画を学んで実装する」素材一式。

**パイプライン**: dance モードに相乗り (Whisper + シーン検出 + 代表フレーム抽出)。差分は `prompts/tutorial.md` のみ。

**PROMPT.md (= tutorial.md) の指示内容**:

1. **題材把握** (`BRIEF.md` に書き出す) — 想定スタック、最終成果物、対象読者
2. **手順書作成** (`TUTORIAL.md`) — 字幕とシーンを論理ステップに分割、各ステップに該当フレーム参照
3. **実装** (`code/`) — TUTORIAL.md を上から順に実装、frames を文字起こしする要領でコード復元、推定箇所は `IMPLEMENTATION_NOTES.md` に記録
4. **自己評価 + Obsidian 用要約** (`REPORT.md`)

**思想**: 「Pythonスクリプトは素材準備のみ、思考は Claude Code に任せる」という vidkit 全体の方針と整合。

**使い方**:
```bash
# YouTube URL でも ローカルファイルでも自動判別
uv run vidkit tutorial https://www.youtube.com/watch?v=xxx
uv run vidkit tutorial ~/Movies/coding-tutorial.mp4

# 生成後
cd ~/Downloads/vidkit_2026-05-19_tutorial_xxx/
claude
> PROMPT.md を読んで実行して
```

## 📤 出力先

デフォルト: `~/Downloads/vidkit_YYYY-MM-DD_<mode>_<id>/`

**`--vault-path` オプション (2026-05-19 追加)**:

```bash
uv run vidkit autocut clip.mp4 --vault-path ~/ObsidianVault
# → ~/ObsidianVault/raw/vidkit/autocut/vidkit_2026-05-19_autocut_clip/
```

`<vault>/raw/vidkit/<mode>/vidkit_<date>_<mode>_<id>/` に出力。
全モード (`dance` / `lecture` / `autocut` / `tighten` / `tutorial`) で動作。

## 🔮 今後の拡張候補

### FCPXML ラウンドトリップ (2026-05-18 採用決定)

詳細は [[2026-05-18_FCPXML_ラウンドトリップ採用]] と [[2026-05-19_vidkit_tighten_tutorial_完成]] を参照。

| オペレーション | 内容 | 状態 |
|-------------|------|------|
| **tighten** | FCPで荒く並べた素材の各クリップ内の残り無音をさらに詰める | ✅完成 (2026-05-19) |
| speaker-filter | 話者ベースで残す/削る (diarization要) | 未着手 |
| marker-batch | トランスクリプトのキーワードでチャプターマーカー一括挿入 | 未着手 |
| beat-snap | 既存カット点を音楽ビートにスナップ (蛹用途寄り) | 未着手 |
| roles-bulk | 全クリップに同じFCP rolesを付与 (ステム分離用) | 未着手 |

tighten 完成時に **FCPXML リーダー (`parse_fcpxml`) + 再シリアライザ (`write_fcpxml_from_parsed`)** が `fcpxml.py` に汎用モジュールとして揃ったので、speaker-filter 以降の追加コストは低い。

### Obsidian Vault hook 連携

`--vault-path` (2026-05-19 追加済み) の自動化:
将来的に Claude Code の hook で「動画処理 → Vaultに自動保存」フローを構築する余地あり。

## ⚠️ 注意点

### Mac環境 (M5 Max 36GB) 向け最適化済み
- 1080p フレーム維持
- large-v3 モデルデフォルト
- 大きなモデルでも余裕

### lecture モードを使うには HF_TOKEN が必要
`.env` ファイルに HuggingFace のトークンを設定 (pyannote使用のため)。
未設定だと話者分離が動かない。詳細手順は `docs/lecture-setup.md` 参照。

### 動画ファイルパスの扱い
日本語・スペース・全角数字・特殊文字を含むパスでも動作することを確認済み (例: `~/Downloads/オンデマンド経営学５-3.mp4`)。
シェルから渡すときは `"..."` で必ず quote する。

### variable-fps 動画 (Zoom録画 / screen capture) の扱い (2026-05-19 fix 済み)
- ffprobe が `r_frame_rate=600/1` のような擬似値を返す動画 (Zoom / Quicktime screen capture / browser MediaRecorder) に対応済
- `fetch._pick_fps()` が r/avg を比較し、`r > 120 かつ avg と 1.0 以上乖離` なら avg を採用
- `fcpxml._pick_frame_duration()` のスナップ許容は **0.5 fps** (30.302 → 30、29.83 → 29.97 を許容)
- それでもスナップできない VFR や 15fps などの非標準値は事前 transcode が必要: `ffmpeg -i input.mp4 -c:v libx264 -r 30 -c:a copy output.mp4`

## 🔗 関連

- [[2026-05-18_FCPXML_ラウンドトリップ採用]] — FCP操作方式の意思決定
- [[2026-05-19_vidkit_tighten_tutorial_完成]] — tighten / tutorial / --vault-path / variable-fps fix の実装記録
- [[claude_code]] (knowledge/programming/tools/) — Claude Code 全般
- **Skill (キーワードで自動ロード)**:
  - `~/.claude/skills/fcp-autocut/SKILL.md` — 単一動画の無音カット
  - `~/.claude/skills/fcp-tighten/SKILL.md` — FCP プロジェクトの再カット (FCPXMLラウンドトリップ)
  - `~/.claude/skills/video-tutorial/SKILL.md` — チュートリアル動画 → 自走実装
- **プロジェクト本体**: `/Users/ittou/projects/vidkit/` / GitHub: <https://github.com/Yitao-Ding/vidkit>
- **ユーザーガイド** (プロジェクト内):
  - `docs/design.md` — 設計書 v2
  - `docs/lecture-setup.md` — HF_TOKEN 取得 5 ステップ
  - `docs/tighten-howto.md` — FCP 実プロジェクトでの tighten 実機検証手順

## 📝 メンテナンス

新機能追加・モード追加のたびにこのファイルを更新する。
意思決定が伴う場合は `decisions/` にも記録。

---

## ✅ うまく行ったこと

### 全体設計
- **uv + Typer の組み合わせ**: パッケージ管理とCLI構築が爆速で進んだ。新しいモード追加も既存パターンを踏襲するだけで済む。
- **asyncio.gather 並列処理**: 4Kの15秒動画を281秒で完走 (シーン検出 + 文字起こし + フレーム抽出が同時並行)。シーケンシャル処理なら倍以上かかってた。
- **モード制で機能拡張**: dance / lecture / autocut が同じCLIで使い分けられる。新機能追加時に既存機能を壊さない。

### dance モード
- **MLX-Whisper (large-v3) が爆速かつ高精度**: M5 Max のNeural Engine をフル活用、CPUより数倍速い。
- **PySceneDetect のシーン検出が想像以上に正確**: ダンスMVの細かいカット (0.3秒の最短カット) も拾えた。
- **4K 60fps 維持で1080pフレーム抽出**: 画質劣化なしで分析素材に変換。

### autocut モード (新規)
- **既存 vidkit インフラの流用**: ffmpeg, pipeline.py, cli.py を流用、ゼロから書いたのは silence.py と fcpxml.py だけ。
- **FCPXML 1.13 シリアライザがバグなく一発で動作**: 有理数時刻計算 (frameDuration 100/3000s) もフレーム整列も完璧。
- **xmllint --noout 通過**: XMLの構造的正しさが保証された。
- **2プリセット (lecture/vlog) で実用カバー率高い**: トーク用 (-30dB / 0.4s) と Vlog用 (-35dB / 0.8s) で大半のユースケース対応。
- **Skill 化で自動ロード成功**: `~/.claude/skills/fcp-autocut/SKILL.md` に置くだけで、FCP/FCPXML/無音カット等のキーワードでClaude Codeが自動ロード。

### Plan エージェント (Opus 4.7) の活用
- 実装前に **設計を Plan エージェントに検証してもらった** → 後の手戻りゼロ。

### tighten モード (2026-05-19 新規)
- **既存ヘルパーの共用**: `_snap_to_frame` を `snap_to_frame` として別名公開するだけで tighten 側から再利用できた。autocut のフレーム整列ロジックを 1 行も書き直さずに済んだ
- **denominator 統一の事前変換**: `parse_fcpxml` が per-element den を sequence den に rebase したおかげで、tighten 側のオフセット計算が「整数の足し算だけ」に収まった。float に落とすバグの余地が消えた
- **ClipRef.extra_attrs で属性 carry-over**: 管理外属性をそのまま辞書に退避する設計で、未来の FCPXML 拡張属性 (lane / audioRole / format / …) に手を入れずに耐性が出た
- **実機 47分動画で冪等性確認**: autocut → tighten で +0.8s しか追加削除されず (0.03%)。tighten が「autocut で詰めた状態」を破壊せず取りこぼし無音だけ拾える挙動が実証された

### tutorial モード (2026-05-19 新規)
- **dance パイプライン相乗り**: 「処理は素材準備のみ、思考は Claude Code 側に任せる」設計思想と整合。新パイプラインを書かずに済んだ
- **`prompts/tutorial.md` だけで完結**: コード変更は `VALID_MODES` / `_template_for` の 2 ヶ所のみ。プロンプトに 4 ステップ自走指示 (BRIEF → TUTORIAL → code → REPORT) を書いて任せる

### --vault-path (2026-05-19 新規)
- **`_resolve_out_root(args)` で 1 ヶ所集約**: 各モードは何も知らずに恩恵を受ける。次に新モード追加するときも自動で対応

### variable-fps 対応 (2026-05-19 fix)
- **Zoom録画 / screen capture / オンライン講義 を救った fix**: `r_frame_rate=600/1` の擬似値問題を avg_frame_rate フォールバックで吸収。実音声 47分の経営学講義で実機検証
- **`_pick_frame_duration` 許容 0.5 fps**: 30 / 29.97 / 25 / 24 は依然区別可能 (互いに ≥1.0 離れている)
- **autocut が 47.5 分動画を 2.25 秒で完走**: ffmpeg silencedetect は I/O 律速。M5 Max の余裕

### GitHub Private 化 (2026-05-19)
- **gh CLI で 1 行 push**: `gh repo create vidkit --private --source=. --remote=origin --push` で repo 作成 + push + origin 設定が同時に終わる。リポジトリ作成スクリプト不要

## ❌ 詰まったこと

### dance モード
- **初回 large-v3 モデルのダウンロードが遅い**: 281秒のうち実質5分はモデルDL。初回だけなので2回目以降は問題ないが、最初焦った。
- **pyannote の HF_TOKEN セットアップが面倒**: HuggingFace でゲート付きモデルのリクエスト承認待ちが発生。lecture モード未完成の主因。

### autocut モード
- **FCPXML の audioRate の型が場所によって違う**: sequence では文字列 "44.1k"、asset では整数 44100。これは FCPXML 1.13 の仕様で、最初は統一しようとしてXMLパースエラーになった。
- **frameDuration の有理数表現**: 30fps NDF は 100/3000s と書く必要がある。1/30 と書くとフレームずれが発生。
- **絶対 file:// URL 必須**: media-rep src を相対パスにすると FCP が見つけられない。

### 一般的な詰まり
- **「FCP内でClaudeが操作」の幻想**: AppleScript はFCPでは事実上使えない、computer-use MCP もピクセル精密制御は脆い → 結局 FCPXML ラウンドトリップしか実用解がない、と気づくのに時間がかかった (詳細: [[2026-05-18_FCPXML_ラウンドトリップ採用]])。

### tighten / variable-fps (2026-05-19)
- **`r_frame_rate=600/1` の罠**: ffprobe が Zoom 録画系の MP4 に対して r_frame_rate を「サンプリングクロックの最大値 (600/1)」として返す仕様に最初気付かなかった。実 fps は `avg_frame_rate=8641100/285163 ≈ 30.30`。固定 fps の動画では問題にならないので autocut セッションでは表面化していなかった
- **`_pick_frame_duration` の許容 0.1 が厳しすぎ**: 30 と 29.97 を区別したい意図で 0.1 にしていたが、screen recording の 30.302 すら弾いてしまっていた。「区別したい最近接ペアの距離」で許容を決めるべきだった
- **長い日本語パスのシェル escape**: `~/Downloads/オンデマンド経営学５-3.mp4` のような日本語+全角数字パスは `"..."` で必ず quote する。`ZSH: no matches found` 系のエラーで一度引っかかった
- **`--vault-path` 動作確認時にテスト出力が Vault 内に残る**: 後で手動 rm が必要だった。テスト出力は `--out /tmp/vidkit_test` に向け、`--vault-path` は本番経路の最終確認だけにする
- **OBJ-C class 重複警告**: `cv2` と `av` の libavdevice が衝突して起動時に warning が出る (動作には影響なし)。autocut セッションから持ち越しの既知問題

## 📋 次回同じことをするときのチェックリスト

### vidkit の新モードを追加するとき

- [ ] **既存パターンを踏襲する**: `cli.py` に新モード追加 → `pipeline.py` に分岐追加 → `modules/` か直下に新モジュール追加
- [ ] **prompts/ に対応プロンプトを置く**: モードごとに使うClaude向け指示
- [ ] **Skill登録するか判断**: 頻繁に使うなら `~/.claude/skills/<name>/SKILL.md` を作る (Claude Codeが自動ロード)
- [ ] **テストクリップを用意**: ffmpegの lavfi で小さな合成テスト動画を作って検証 (実動画前)
- [ ] **xmllint で検証**: XML出力するモードなら必須
- [ ] **プリセットを2つ以上用意**: 1つだとカスタマイズが面倒、3つ以上だと選択肢過多
- [ ] **dance 系パイプラインに相乗りする場合**: `VALID_MODES` 追加 + `prompts/<mode>.md` + `_template_for` 分岐の 3 点のみ。新パイプラインを書かない (tutorial が好例)

### FCPXML ラウンドトリップで新オペレーションを追加するとき (2026-05-19 確立)

- [ ] `parse_fcpxml` の戻り値 `ParsedFCPXML` を出発点にする (新パーサを書かない)
- [ ] **時刻計算は全部 numerator (整数) ベース**。秒に落とすのは ffmpeg に渡す瞬間と人間向け表示だけ
- [ ] 既存 `snap_to_frame` を使って source-time を frame 整列させる
- [ ] 新オペレーションは `vidkit/<op>.py` に独立モジュールとして置く
- [ ] CLI 統合は `VALID_MODES` 追加 + `pipeline.run()` の分岐 + `_run_<op>` 関数の 3 点だけで済むよう設計
- [ ] **合成 FCPXML (`write_fcpxml` で 1 clip 直書き) + 実動画の autocut 出力 + 実講義動画 の 3 パターン**で検証
- [ ] `xmllint --noout` 必ず通す
- [ ] 「未知 asset / 行方不明 src」のフォールバックを最低限決めておく (pass-through or drop)

### variable-fps / 非標準 fps の動画を vidkit に通すとき

- [ ] **ffprobe で `r_frame_rate` と `avg_frame_rate` の両方を見る**。Zoom録画 / screen capture / browser MediaRecorder は r が 600/1 のような擬似値を返す
- [ ] `r > 120 かつ avg と 1.0 以上乖離` していたら **avg を信用** (`fetch._pick_fps()` 自動)
- [ ] スナップ許容は **0.5 fps**。30.302 → 30、29.83 → 29.97 のように寄せる。FCP に 30 として渡しても映像/音声タイミングは劇的にはずれない
- [ ] それでも snap できない fps (15 / 100 / 任意 VFR) は **事前 transcode** が必要: `ffmpeg -i input -c:v libx264 -r 30 -c:a copy output.mp4`

### autocut を本番動画で使うとき

- [ ] 動画ファイルパスに**日本語/スペース/特殊文字が含まれてないか**確認 (一応動くが、念のため)
- [ ] プリセット選択: トーク・講義系 → `lecture`、Vlog・インタビュー → `vlog`
- [ ] 必要なら閾値カスタマイズ: `--silence-threshold` (dB)、`--min-silence` (秒)、`--pad` (秒)
- [ ] 出力された FCPXML は **新規プロジェクトとして** FCP にインポート (既存に上書きしない)
- [ ] FCPで開く: `open -a "Final Cut Pro" /path/to/autocut.fcpxml`
- [ ] **FCPの自動起動はしない仕様**なので、起動忘れに注意

### FCPXML 周りで新機能を作るとき

- [ ] `audioRate` の型: sequence は文字列、asset は整数 (混同すると壊れる)
- [ ] `frameDuration` は有理数表記: 30fps NDF なら `100/3000s`、24fps なら `100/2400s`
- [ ] `media-rep src` は**必ず絶対 file:// URL**
- [ ] `offset` は前クリップの累積和、`start` は元動画内の開始時刻、`duration` はクリップの長さ
- [ ] フレーム整列: `duration` は frameDuration の整数倍にする
- [ ] 検証: `xmllint --noout` で構造的正しさ確認 + 実機 FCP でインポートテスト

### lecture モードを完成させるとき (将来)

- [ ] HuggingFace で pyannote/speaker-diarization-community-1 のリクエスト承認を待つ (数時間〜数日)
- [ ] `.env` に `HF_TOKEN=<token>` を設定
- [ ] テストクリップで動作確認: 複数話者音声を用意
- [ ] プロンプト (lecture_brief.md, standard.md, deep.md) を実講義音声で調整

---

## 🔗 関連 (再掲)
