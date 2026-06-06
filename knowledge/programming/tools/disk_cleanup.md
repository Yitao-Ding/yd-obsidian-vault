---
type: knowledge
domain: programming/mac
created: 2026-06-02
tags: [mac, storage, cleanup, maintenance]
---

# Mac ストレージ整理 手順 (YD環境)

> 2026-06-02、YDのMac (1.8TB, 93%使用/残130G) を整理して224.7G回収した時の知見。
> 関連: [[mac_automation]] / [[claude_mistakes]] (A-16) / log.md 2026-06-02

## 結論 (TL;DR)

YDのMacは**ディスクの大半が映像/写真の制作データ(900G超)**。ゴミ掃除で取れるのは150〜290G程度。
根本解決は**完了案件を外付けSSDへオフロード**。掃除と offload は別問題として扱う。

手順: ①読み取り専用で全カテゴリ並列監査 → ②安全/要判断/オフロードに分類 → ③YD承認 → ④`rm`直接削除(ゴミ箱経由は同一ボリュームで容量空かない)。

## 回収できた内訳 (2026-06-02 実績)

| 項目 | 回収 | 分類 | 復元方法 |
|---|---|---|---|
| World of Tanks ASIA | 124.6G | CrossOverゲーム | Wargaming Game Centerから再DL |
| LM Studio 重複quant (Qwen3.5-35B Q6_K) | 26.6G | redownloadable | HuggingFaceから再DL。Q4_K_M側を保持 |
| Steam Liftoff Micro Drones | 19.9G | redownloadable | Steamから再DL (appmanifest_*.acfも削除) |
| Downloads 重複インストーラ | 16.8G | installer-junk | **DaVinciの.dmg+.zip重複14.9Gが最大**。全て/Applicationsにインストール済確認後 |
| 各種キャッシュ | 14.8G | cache | npm/uv/pnpm/brew/ブラウザ/ShipIt(適用済) |
| App Support Electronキャッシュ | 11.1G | cache | Discord旧版/Canva/Miro/Notion Partitions/Figma/Chrome SW |
| Xcode DerivedData + 旧iOSシンボル | 9.1G | dev-artifact | DerivedData自動再生成。古いDeviceSupportは接続時のみ再DL |
| ログ/アーカイブ済ビルド | 1.8G | cache | Adobe/CC/各種ログ + projects/_archive のnode_modules |

## 大容量の在処 (YD環境固有のマップ)

- `~/YITAO's FILE` 746G — 制作データ本丸。ジジババRunaway 382G(4/3以降ノータッチ)/2025写真216G/ドローン96G
- `~/Downloads` 238G — うち `02_映像写真プロジェクト` 209G が制作素材(Downloadsに置きっぱが根本原因)
- `~/Library/Application Support` 181G — うち **World of Tanks 125G** が69%。Docker/iPhoneバックアップは無し
- `~/Music` — Logic音源は `~/Music/Logic Pro Library.bundle` ではなく **`/Library/Application Support/Logic` (58G, sudo必須)**。EXS Factory Samples 44G + Alchemy 14G。`/Library/Audio/Apple Loops` 6G も
- `~/Library/Containers` 50G — Hasselblad Phocus RAW 24G + GoodNotes 16G(実コンテンツ、消すな)。PowerPointキャッシュ5Gは安全
- `~/.lmstudio` 72G — LMモデル。同一モデルの複数quant重複に注意

## ✅ うまく行ったこと

- **読み取り専用の並列監査を先にやった**。9エージェントでカテゴリ別に `du` 走査+分類、削除は一切させない設計。これで「何が安全/何が要判断」が正確に出て、YDに数字で選んでもらえた
- **df -k のAvail差分でフェーズ毎の回収量を計測**。`du`を消す前に全部やると遅いので、df差分が最速かつ正確
- **ゴミ箱経由(`mv ~/.Trash`)を使わず`rm`直接**。同一APFSボリュームではゴミ箱に移しても容量は空かない(空にするまで)。容量逼迫時は`rm`一択
- インストーラは `/Applications` に対応アプリがあるか確認してから削除 → 安全に16.8G

## ❌ 詰まったこと

- **監査エージェントが Logic 音源のパスを誤報告** ("`~/Music/Logic Pro Library.bundle/Samples` 55G")。実際そのbundleは1.7Gのパッチ集で、本体は `/Library/Application Support/Logic` (58G, sudo必須)。`rm`したら freed 0G で発覚 → 実体を `find`/`du` で再確認。教訓は [[claude_mistakes]] A-16
- `~/Music` が監査時97G→整理後37Gに見えたが、df差分(224.7G)は自分の削除と完全一致。Apple Musicのpurgeable/最適化ストレージによる計測ゆらぎで、実害なし。**df を真とする**
- Logic音源は `/Library` 配下で sudo 必須 → Claude Codeの非対話Bashでは `sudo -n` が通らず実行不可。YD本人にコマンド委譲(`! sudo rm -rf ...` かGUIのSound Library Manager)

## 📋 次回チェックリスト

1. まず `df -h /System/Volumes/Data` で逼迫度確認、`du -sh ~/* ~/.[^.]* | sort -rh | head -35` でトップレベル把握
2. **削除候補は必ず `rm` 直前に実体を `du`/`ls` で確認**(サブエージェントやメモリのパスを鵜呑みにしない、特に `.bundle` や `/Library` 系)
3. 大容量の在処は上記マップ参照。`/Library/Application Support/Logic` (音源) と `World of Tanks` (125G) は YD環境の二大隠れ容量
4. 容量を空けるなら**ゴミ箱経由NG**、`rm`直接。フェーズ毎に `df -k` Avail差分で回収量を出す
5. App Support/Containers のアプリキャッシュは**起動中だと不整合の可能性** → 消したら該当アプリ再起動を案内(データ消失はなし)
6. `/Library` 配下(Logic音源/GarageBand/Apple Loops)は **sudo必須 → YDに委譲**。Claude Codeの非対話シェルでは sudo 実行不可
7. 制作データ(映像/写真)は**絶対に勝手に消さない**。offloadは外付けSSD前提でYD判断。掃除とoffloadは別タスク
8. node_modules/.venv/.next は再生成可だが、稼働中プロジェクトは次回 `npm install`/`uv sync` が要る → 「安全だが手間あり」枠として別提示

## 外付けドライブ(映像/写真アーカイブ)の重複整理 (sv q, 2026-06-02)

内蔵と違い外付けは**重複が容量の主因**。映像は1ファイル数十GBなので「重複1グループ消すだけで数十G」。sv q(1.8TB/94%)で316G回収した時の手順:

- **既存の重複リストがあれば全再ハッシュせず再利用**。`重複ファイル一覧_YYYY-MM-DD.txt`(過去のdedupツール出力)を実ファイルと **size 突き合わせ**するだけで「今も健在の重複」が秒で出る。sv q は5/11リスト132グループ中131が健在だった
- **削除は必ず `cmp -s` でバイト一致を確認してからのみ `rm`**。size一致だけでは消さない(安全弁)。これで誤削除0件。irreplaceable な素材には必須
- **どっちのコピーを残すかはフォルダ意味論で決める**。例: 「Day2/Day3」で末尾だけ重複=後発(Day3)側を消しDay2維持 / 「`exiftool/`配下」=派生コピーなので消す / 「採用」=「元素材」に同一実体があれば採用側を消す(※"選抜した事実"のフォルダ分け情報は失う=YD事前合意必要)
- **`.dra`(DaVinciアーカイブ)を消す前に中の orphan を救出**。`.dra/MediaFiles/` 内の各ファイルを生素材フォルダと突き合わせ、**生側に無いファイル(購入Artlist素材・ロゴ・録画等)を先に同一ボリューム `mv` で退避**(一瞬・容量消費0)。sv q では102.8G中102.1Gが生素材と重複、固有0.7G(49件)だけ救出して.dra全削除
- **削除後に部分ハッシュで全ドライブ再スキャン**して取りこぼし確認。先頭4MB+末尾4MB+size の sha1 を **同サイズ群のみ**計算=1.8TBでも数分。sv q は再スキャンで50MB級重複0件=クリーン確認できた
- 「一時的」「temp」名のフォルダは中身がカメラ吸い出しの一時置きなことが多い(sv qは120G全部Sony吸い出し)。ただし**他にバックアップがあるかは必ずYDに確認**してから消す
