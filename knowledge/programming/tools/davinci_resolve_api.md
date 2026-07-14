---
type: knowledge
area: programming/tools
created: 2026-07-14
source: ダンちゃり長野ドキュメンタリー制作 (2025-07-06_Nagano/davinci/、minitest/REPORT.md + build/BUILD_LOG.md の実測)
---

# DaVinci Resolve スクリプトAPI 実戦ノート (Studio 21)

> 2026-07-14、長野ドキュメンタリー2本 (4K・全字幕焼き込み) を Python API でゼロから構築・レンダーした実測知見。
> 一次資料: `/Volumes/Extreme pro/ダンちゃり_ALL/動画/2025-07-06_Nagano/davinci/minitest/REPORT.md` (API罠の対照実験) と `davinci/build/BUILD_LOG.md` (本実装ログ)。

## 接続ボイラープレート (実証済み)

```python
import os, sys
os.environ["RESOLVE_SCRIPT_API"] = "/Library/Application Support/Blackmagic Design/DaVinci Resolve/Developer/Scripting"
os.environ["RESOLVE_SCRIPT_LIB"] = "/Applications/DaVinci Resolve/DaVinci Resolve.app/Contents/Libraries/Fusion/fusionscript.so"
sys.path.append("/Library/Application Support/Blackmagic Design/DaVinci Resolve/Developer/Scripting/Modules")
import DaVinciResolveScript as dvr
resolve = dvr.scriptapp("Resolve")  # Resolve起動中のみ。Studio判定は resolve.GetProductName()
```

## ✅ うまく行ったこと

- **PNG→ProRes4444(alpha)焼き込みでテロップ全部を回避実装**: Text+ を一切使わず、Pillow (ヒラギノ .ttc を index 指定でロード: 明朝ProN W3=idx0 / W6=idx2) で透過PNG → ffmpeg `prores_ks -profile:v 4444 -pix_fmt yuva444p10le` でフェード焼き込み → `AppendToTimeline` の `recordFrame`+`trackIndex` で正確配置。フォント問題・挿入位置問題・数量スケール問題 (テキスト88本) を全て回避。
- **音は ffmpeg 完全ミックス1本方式**: Fairlight はAPI不可のため、現場音 (dialog正規化 -19LUFS±8dB) + BGM (区間ゲイン+フェード) を1本のwavに焼いて A1 配置。映像クリップは `mediaType:1` (映像のみ) で置くと内蔵音声がタイムラインに入らず衝突しない。
- **Resolveは映像だけレンダーして音はffmpegでmux**: Resolve の AAC は True Peak が上振れする (ミックスwav TP-1.6 → プレビューmp4 TP-0.8)。最終は `ExportAudio=False` で映像のみ書き出し → master wav から `-c:v copy` で AAC 320k mux。TP≤-1.0 を確実に満たせる。
- **フレーム計算**: `AppendToTimeline` の startFrame/endFrame は**ソースのネイティブfps基準** (DJI 59.94p素材は59.94基準)。59.94→29.97タイムラインは実時間保持の単純間引き。native span を偶数に丸めるとタイムライン尺が厳密整数になり、配置後の終端読み戻し検証が1フレームもズレない。
- **検証駆動**: 全 append 後に `GetItemListInTrack` で終端を読み戻し設計値と照合 → 45クリップ×2本でズレ0。

## ❌ 詰まったこと

1. **Text+ のフォント名はウェイト込み表記が必須で、間違いは"レンダリング時にのみ"失敗する**: `SetInput("Font", "Hiragino Mincho ProN")` は成功したように見え `GetInput` 読み戻しも一致するが、レンダーが「Fusionコンポジションを処理できませんでした」で失敗。`"Hiragino Mincho ProN W3"` なら成功。失敗時の出力は **3,786バイトの空mp4** (自動判定に使えるシグネチャ)。どのツールが原因かはエラーに一切出ない。
2. **APIの戻り値は成否判定に使えないものが多い**: `SetSetting`/`SetInput` は成功しても None を返す → 対応する Get で読み戻し検証が唯一の方法。`CreateProject`/`CreateEmptyTimeline`/`AddRenderJob` はタイミングでまれに None → retry(5回,1s) で解決。
3. **Blackmagic Cloud のプロジェクトライブラリはセッションが切れると PM API が全 None 化**: `GetCurrentDatabase()`=None が決定的兆候。**GUI で Project Manager を開くと再接続される** (スクリプトからは復旧不可)。`SetCurrentDatabase(クラウドDB)` も API では False (GUI認証依存)。`pm.DeleteProject` もクラウドでは常に False。→ **長時間の自動ビルドはローカルDBで行い、.drp で引き渡すのが正解**。ライブラリをGUIで切り替えると開いているプロジェクトは閉じる (保存済みなら安全)。
4. **`AppendToTimeline` は recordFrame 未指定だと「タイムライン全体の現在編集位置」に置かれる** (トラックの空き先頭ではない)。音声・オーバーレイは `AddTrack` + `recordFrame` + `trackIndex` を必ず明示し、配置後 `GetStart()` 検証。
5. **同一トラックに時間重複するクリップは置けずサイレント失敗** → 重複するテロップ (字幕+補足) は貪欲区間分割で V3/V4… に層分け。
6. **細かい罠**: `project.SaveProject()` は存在しない (`pm.SaveProject()`)。`DeleteClips` は複数トラック混在バッチでサイレント失敗 → トラック毎に分割。`InsertFusionTitleIntoTimeline` は隙間を優先して埋める。スクリプト終了時にまれに SIGSEGV (exit 139) するが処理自体は完了済みで無害 (判定は stdout マーカーで)。mp3 を filtergraph で扱う時は `aresample=48000` を先頭に (44.1kHzのままだと atrim のサンプル数計算がズレる)。

## 📋 次回同じことをするときのチェックリスト

1. Resolve 起動 → `scriptapp("Resolve")` → `GetProductName()` で Studio 確認 → `GetCurrentDatabase()` が None でないか確認 (None なら GUI で PM を開く)
2. **ビルド先はローカルDB** (`SetCurrentDatabase({'DbType':'Disk','DbName':'Local Database'})` は API 可)。終了後のクラウド復帰は GUI で
3. プロジェクト設定 (fps/解像度) は**タイムライン作成前**に SetSetting → GetSetting 読み戻し
4. テロップが多い/フォント厳密なら最初から PNG→ProRes4444 焼き込み方式を選ぶ (Text+ は1〜2枚の単発タイトルまで)
5. Text+ を使うなら: フォント名はウェイト込み + **必ず1回実レンダーして出力サイズを確認** (数KBなら失敗)
6. 音声設計は「ffmpeg で完成ミックス1本 → A1」一択。映像クリップは mediaType:1
7. フレーム計算関数は共通モジュール化し、ソースネイティブfps基準 + DJI偶数span丸めを徹底
8. 「状態を作る」系API (CreateProject/CreateEmptyTimeline/AddRenderJob/AddTrack) は全部 retry でラップ
9. 最終書き出しは映像のみResolve + 音声ffmpeg mux (ラウドネス実測: I=-14±0.5 / TP≤-1.0)
10. これだけは忘れるな: **戻り値を信用しない。読み戻して検証。レンダーして確認。**

## 関連

- [[vidkit]] / [[localhost_fm]] (ffmpeg ラウドネス処理の先行知見)
- 実装一式 (再利用可能なパイプライン): `2025-07-06_Nagano/davinci/build/` の common.py / gen_manifest.py / render_texts.py / build_mix.py / build_timeline.py
- [[2026-07-14_長野ドキュメンタリー_全字幕方式完成]] (意思決定記録)
