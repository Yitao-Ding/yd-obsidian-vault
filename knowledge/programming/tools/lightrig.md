---
type: knowledge
area: programming/tools
created: 2026-07-11
last_updated: 2026-07-14 (v1.1 反映)
---

# LightRig — スタジオ照明セッティング記録アプリ

> スタジオで組んだ照明 (機材・位置・出力・色) を 3D で記録し、撮影写真と紐付けて後から見返す YD 個人用アプリ。
> 「照明は文字でメモできない」問題の解。1セッション (2026-07-11) で設計→実装→実機導入まで完了。

## 概要

- **場所**: `~/AI projects/Lighting` (正本 `HANDOVER.md` / 設計 `DESIGN.md` / 使い方 `README.md`)
- **対応**: iPhone / iPad / Mac (SwiftUI マルチプラットフォーム、iOS 17+ / macOS 14+)
- **同期**: iCloud (SwiftData + CloudKit)。サーバー・追加コスト0。iCloud 未サインイン時はローカルに自動フォールバック
- **記録単位**: セッティング → ライトN灯 (機材ID / 位置・高さ・pan/tilt / 出力% / 色温度K / ジェル sRGB / モディファイア11種 / ラベル) + 写真N枚 (アプリ内JPEGコピー、写真ライブラリ権限不要) + メモ
- **機材カタログ**: 49機種実スペック (Profoto 9 / Godox 13 / Aputure 8 / Amaran 7 / Nanlite 7 / Generic 5)。追加は `LightRig/Shared/Catalog/fixtures.json` に1エントリ足すだけ

## 使い方・運用

- 導入: `open "~/AI projects/Lighting/LightRig.xcodeproj"` → scheme (LightRig-iOS / LightRig-macOS) + 端末選択 → ▶。証明書は約1年有効、年1回▶し直し
- 操作 (v1.1): ライト/レフ板は**そのままドラッグで移動** (選択不要) / 1タップ=選択 / **2タップ=インスペクタ** / 空ドラッグ=半球ドームカメラ (横=周回・縦=上下、床が下限、注視点は被写体固定) / ピンチ=ズーム / 右下ボタンで視点プリセット (正面/45°/俯瞰/リセット)
- レフ板: +メニュー→レフ板を追加 (白/黒/カスタム色、幅・高さ 0.3〜3.0m)。背景紙は既定で壁全幅、部屋はプリセット (小5×4/標準8×6/大12×9)
- プレビュー光量は見た目チューニング値 (`ColorMath.previewLumens`: LED W×8 / ストロボ Ws×6)。**保存データは生値** (%・K・hex) なので描画を変えても記録は不変
- 3D は SceneKit (WWDC25 deprecated だが Xcode 26 で完全動作)。将来 RealityKit 移行は `Scene/` 2ファイル置換で済む設計

## ✅ うまく行ったこと

- **契約駆動の並列実装**: Fable が DESIGN.md + コア6ファイル (型・モデル・数式) を実コードで先に固定 → Sonnet 5体が別ファイルを並列実装 → Opus 統合が**ほぼ無修正で結合** (修正はスタブ削除と @StateObject 化の2点のみ)。「契約はプロンプトでなく実コードで渡す」が効いた
- **検収→修正ループの分離**: Fable がスクショを直接検分して問題を言語化 (白飛び/形状/照射方向) → Sonnet に「自分でスクショを撮って目視→修正」のループを持たせたら6ラウンドで合格品質に
- **PhotosPicker のアプリ内コピー方式**: 権限ゼロ・localIdentifier 非依存・CloudKit で写真ごと同期、の3問題を一挙に回避
- XcodeGen で Xcode プロジェクトを CLI 完結生成 (`project.yml` → `xcodegen generate`)

## ❌ 詰まったこと

- **Max セッション上限でワークフロー全滅** (resets 3:20am)。リセット後に同一スクリプトで再走して解決 — Workflow はスクリプトが残るので再走コストほぼゼロ
- **チームIDの誤推測**: 証明書 CN の括弧内 (L55GP6S566) は個人識別子。正 = OU フィールド (FC2V887B8C)。詳細 [[claude_mistakes]] B-7
- **Apple PLA 更新の未同意**で provisioning 全滅 (「PLA Update available」)。本人が developer.apple.com で同意 → 反映されない時は Xcode Accounts でアカウント削除→再サインイン
- **simctl 無応答はランタイムDL中だった**: 「CoreSimulator ハング」に見えたが実は iOS 26.5 ランタイムを自動DL中 (数十分)。壊れたと決めつけず Install ログを待つ
- **NSClickGestureRecognizer に `require(toFail:)` は無い** (UIKit 専用 API)。macOS のシングル/ダブルクリック共存は「単クリック=選択が先に走り、ダブル成立時にインスペクタを追加で開く」設計で抑制不要にするのが正解 (v1.1 統合で修正)
- v1.1 の教訓: **実機を触った本人の操作FBが最速の仕様書**。「選択→メニュー出っぱなしでドラッグ」の不便さはスクショ検分では見えなかった。操作系は初回から「直接操作 (ドラッグ即移動) + 詳細は明示アクション (2タップ)」に寄せるべき
- SceneKit の光量: 実 lm/W をそのまま入れると白飛び (LED 110→8 に減衰)。SCNVector3 は iOS=Float / macOS=CGFloat で成分型が違う

## 📋 次回同じことをするときのチェックリスト

1. Apple 系ネイティブアプリを CLI で作る前に: Xcode 有無 (`xcodebuild -version`) / チームID (`security find-certificate ... | openssl x509 -noout -subject` の **OU=**) / シミュレータランタイム (`xcrun simctl list runtimes`、無ければDLに数十分)
2. YD の Xcode GUI 操作前に、**署名込みビルドを CLI で1回検証** (`-destination 'generic/platform=iOS' -allowProvisioningUpdates`) — エラー文がそのまま取れて往復が減る
3. SwiftData + CloudKit のモデル制約: 全プロパティにデフォルト値 / relationship optional / `.unique` 禁止 / macOS は CloudKit.framework 明示リンク
4. マルチエージェント実装は「コア契約を実コードで固定 → ファイル所有権を明記 → 統合1体」の3点セット
5. 「これだけは絶対忘れるな」: `externalStorage` の CKAsset 同期は公式仕様に明記なし — **写真付きデータの2台同期は必ず実機確認**

## 関連

- [[active_projects]] / [[claude_mistakes]] (B-7) / `~/AI projects/Lighting/HANDOVER.md`
