---
type: current_state
last_updated: 2026-07-01
update_frequency: 意思決定のたび
---

# 最近の重要な意思決定

> 「なぜこう決めたか」を残すことで、未来のClaudeと未来のYDが同じ議論を蒸し返さないようにする

## 2026-05-18

### FCP 自動編集の手段として FCPXML ラウンドトリップを採用
- **背景**: vidkit autocut 完成直後、YDが「FCPX内でClaudeが自動操作できないか」と質問。FCP 自動編集の現実解を検討
- **選択肢**:
  - A. computer-use MCP で GUI 操作 (画面ピクセル単位)
  - B. AppleScript (macOS 伝統)
  - C. FCPXML ラウンドトリップ (Export XML → 変換 → Import XML)
- **決定**: **C** (FCPXMLラウンドトリップ)
- **理由**:
  - A はタイムライン上のピクセル精密制御が脆い、FCP のレイアウト変更で壊れる
  - B は Apple が FCP X のスクリプティング機能を意図的に削っているのでほぼ無理
  - C は Auto-Editor / Recut / Trint など業界の自動化ツールが全部採用している実績ある方式
  - 既存 vidkit autocut が一方向 FCPXML 生成を実装済みで、双方向に拡張する形が自然
- **第一弾オペレーション**: tighten (既存FCPプロジェクトの各クリップ内の残り無音をさらに詰める)
- **詳細**: [[2026-05-18_FCPXML_ラウンドトリップ採用]]

### vidkit に autocut モード追加 (FCP用無音カット FCPXML 生成)
- **背景**: トーク/講義/Vlog/インタビュー動画をFCPで本編集する前に無音を機械的にカットしたかった
- **決定**: vidkit に `autocut` モードを追加 (dance/lecture と並列のモード)
- **構成**: ffmpeg silencedetect → keep-segment 計算 → FCPXML 1.13 シリアライザ → `~/Downloads/vidkit_YYYY-MM-DD_autocut_<id>/autocut.fcpxml`
- **プリセット**: `lecture` (タイト: -30dB/0.4s) と `vlog` (緩め: -35dB/0.8s)
- **Skill登録**: `~/.claude/skills/fcp-autocut/SKILL.md` で FCP関連キーワードを自動トリガー
- **検証**: 12秒テストクリップで両プリセット動作確認、xmllint通過、フレーム精度の有理数時刻計算成功
- **詳細**: [[vidkit]] の `autocut モード` セクション

### Obsidian Vault を「Claudeの外部記憶」として構築
- **背景**: Claudeの記憶喪失問題に毎回悩まされていた。メモリ機能だけでは不十分
- **選択肢**:
  - A. メモリ機能をうまく使い続ける
  - B. Notion をハブにする (既にMCP接続あり)
  - C. Obsidian + LLM Wiki パターン (Karpathy方式 + 堀口英剛氏のMistakes方式)
- **決定**: **C を採用**
- **理由**:
  - メモリは中身が見えない・他AIと共有不可
  - Notion はネット必須・他AIから直接読みづらい
  - Markdown ベースの Obsidian なら全AIから読める、ローカル完結、Git でバージョン管理可
  - 5分でセットアップ可能な確立されたパターン (2026年4月公開)
- **規模**: C (全部1Vault) を採用、機密のみ別Vault
- **同期**: Git + GitHub Private リポジトリ
- **言語**: 日本語ベース

### vidkit を Python + uv で構築
- **背景**: 動画 → Claude Code が分析できる素材に前処理するCLIが必要
- **決定**: Python + uv (パッケージ管理) + Typer (CLI) + asyncio (並列処理)
- **理由**: MLX-Whisper、pyannote.audio が Python エコシステム

### vidkit の出力先は `~/Downloads/vidkit_YYYY-MM-DD_<mode>_<id>/`
- **理由**: Mac の Downloads は誰でも見つけられる、後で整理しやすい
- **将来**: Obsidian Vault の `raw/transcripts/` への直接出力オプション追加予定

## 2026-05-17

### Lecture Hub を本番デプロイ
- **背景**: 個人ナレッジハブが欲しかった
- **スタック**: Next.js + Supabase + BlockNote + AI SDK
- **理由**: Task Hub の Firebase に対して、Supabase も使ってみたかった + Authが手軽

## 2026-05-13

### Lightroom Classic 連携の方針
- **背景**: 撮影写真のレタッチを Claude に任せたかった
- **決定**: 直接連携は現状無理 (RAW非対応、Develop操作API非公開)
- **代替**: 自分でレタッチ + Claude にカラーグレーディング方針を相談

## 2026-04-13

### Task Hub の本格商用化検討
- **背景**: 大学サークル向けタスク管理アプリ
- **決定**: フリーミアムモデルで全機能搭載、Salamatで実運用 → 外部β → 広告
- **方針**: 「動くもの」より「ちゃんとしたプロダクト」を作る

## 2026-04-09

### Task Hub のストレージは Firebase (NASは不採用)
- **背景**: 家のNASに置けないかと一瞬考えた
- **決定**: NASは商用プロダクトには不適 (可用性、スケール、セキュリティ)

## 2026-04-01

### Claude の応答スタイルをメモリに登録
- **背景**: 何度も同じスタイル指示を繰り返していた
- **決定**: 「距離感近めの敬語、先輩↔後輩関係」をメモリ化
- **現在**: このVaultの `identity/preferences.md` に詳細を記述

## 2026-03-25

### Salamat WBSチームのHP制作プロジェクト発足
- **背景**: 公式HPがなく、法人化の障害になっていた
- **チーム編成**: YD (リーダー、実装) + Riko (内容) + Haruka (構成) + Rena (デザイン)
- **ツール**: Wix
- **目標**: 5月1日公開

## 2026-03-19

### Arte Grow の事業モデルを Type B に決定
- **背景**: フィリピン現地で何をやるか議論
- **選択肢**: A (依存型 = 現地で作らせて売る) / B (誇り型 = 素材ブランド化) / C (混合)
- **決定**: **B (Pride, Not Dependency)**
- **理由**: 長期的な自立を促す、現地に依存しない仕組み

## 2026-07-01

### 写真セレクト納品フローを確立＋スキル化 (photo-selection-sheet)
- **背景**: 撮影データから客にレタッチ用10枚を選んでもらう。連写重複は省きたいが、ポーズ/表情/構図が違うカットは残したい。低画質PDFでは表情が判断できない
- **選択肢**: A.シャープさ自動選別 / B.連写(EXIF時刻)で重複だけ除去＋人が最終判断＋ズーム対応PDF / C.全カットを客に丸投げ
- **決定**: **B**。一連の流れを再利用スキル `photo-selection-sheet` にパッケージ
- **理由**: 連写(時刻近接)だけ潰せば別カットを守れる／表情差は機械で判定不可なので人が決める／客が拡大して表情確認できるフル解像度個別埋め込みPDF(reportlab)が必須
- **その後**: 498→(JPG139自削除)→359→母集団126→手動209採用→客配布PDF(190MB/14頁)。詳細 [[写真納品セレクトフロー]] / [[2026-07-01_写真セレクト納品フロー確立_スキル化]]

## 📝 意思決定の記録フォーマット

```markdown
### <タイトル>
- **背景**: <なぜこの議論になったか>
- **選択肢**: <検討した案>
- **決定**: <選んだもの>
- **理由**: <選んだ根拠>
- **その後**: <実施結果や反省>
```

## 📚 関連

- [[active_projects]]
- [[open_questions]]
- `decisions/` - 各意思決定の詳細ファイル
