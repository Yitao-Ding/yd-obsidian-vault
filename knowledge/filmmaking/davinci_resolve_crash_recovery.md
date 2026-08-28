---
name: davinci-resolve-crash-recovery
description: DaVinci Resolve が起動クラッシュした場合の診断・復旧手順。Project.db(SQLite)の健全性確認、クラッシュ原因の切り分け、設定リセット方法。
type: knowledge
created: 2026-05-18
project: Hi,Me:) さなぎ DaVinci復旧調査
---

# DaVinci Resolve 起動クラッシュ診断・復旧

> 実績: 2026-05-18 Hi,Me:) 「さなぎ」プロジェクト調査で体系化
> 環境: DaVinci Resolve 21.0.0b.0020 / macOS 26.4 / Apple M5 Max

## 前提: データとアプリを切り分けて考える

「プロジェクトが壊れた」と感じていても、多くの場合 **Project.db(SQLite) 自体は健全で、DaVinciアプリの起動挙動が壊れているだけ**。まずデータが生きているか確認してから対処する。

## ステップ1: DaVinci関連ファイルの全体把握

```bash
# プロジェクト格納先 (Blackmagic Cloud同期型)
ls ~/DaVinci\ Resolve\ Media/

# ローカルプロジェクトDB
ls ~/Library/Application\ Support/Blackmagic\ Design/DaVinci\ Resolve/Resolve\ Project\ Library/Resolve\ Projects/Users/guest/Projects/

# .drb (自動バックアップ) と .drp (エクスポート) を探す
find ~ -name "*.drb" -type f 2>/dev/null
find ~ -name "*.drp" -type f 2>/dev/null

# クラッシュログ確認
tail -100 ~/Library/Application\ Support/Blackmagic\ Design/DaVinci\ Resolve/crash_archive.txt
```

## ステップ2: Project.db の健全性確認

```bash
# SQLiteマジックバイト確認 (先頭が "SQLite format 3" なら形式OK)
xxd ~/DaVinci\ Resolve\ Media/<プロジェクト名>/Project.db | head -5

# 整合性チェック (okが返れば編集データは生存)
sqlite3 ~/DaVinci\ Resolve\ Media/<プロジェクト名>/Project.db "PRAGMA integrity_check;"

# テーブル一覧でDaVinci構造が入っているか確認
sqlite3 ~/DaVinci\ Resolve\ Media/<プロジェクト名>/Project.db ".tables"

# タイムライン数・名前
sqlite3 ~/DaVinci\ Resolve\ Media/<プロジェクト名>/Project.db "SELECT COUNT(*) FROM Sm2Timeline;"
sqlite3 ~/DaVinci\ Resolve\ Media/<プロジェクト名>/Project.db "SELECT Name FROM Sm2Timeline;"
```

`integrity_check` が `ok` = 編集データは無傷。以降はアプリ側の問題を疑う。

## ステップ3: クラッシュ原因の切り分け

```bash
# クラッシュ直前のDaVinci動作ログ
tail -100 ~/Library/Application\ Support/Blackmagic\ Design/DaVinci\ Resolve/logs/davinci_resolve.log
tail -80 ~/Library/Application\ Support/Blackmagic\ Design/DaVinci\ Resolve/logs/ResolveDebug.txt
```

### 既知のクラッシュパターン

**パターン A: 起動時自動オープンでのFusion/OFXクラッシュ**
```
Assertion failed: (false), function Load, file Sm2EffectKeyframe.cpp, line 175
Database transaction is ongoing, user initiated action Sync Asset Map is postponed
```
原因: 前回開いていたプロジェクトのFusion/OFXパラメータ読み込みで失敗。
対処: 対象プロジェクトフォルダを一時リネームして起動回避 → Project Manager画面まで進めてからImport。

**パターン B: macOS×ベータ版DaVinciのクラス重複**
```
Class IMSMaskMetadata is implemented in both /System/Library/Frameworks/ImmersiveMediaSupport.framework
and /Applications/DaVinci Resolve/DaVinci Resolve.app/.../ImmersiveVideoToolbox.framework
This may cause spurious casting failures and mysterious crashes.
```
原因: macOS同梱の `ImmersiveMediaSupport` とDaVinci内蔵 `ImmersiveVideoToolbox` が同一クラスをダブル定義。ベータ版DaVinci × 最新macOS × M5系の組み合わせで頻発。
対処: 安定版DaVinciに切り替え (21.0正式版 or 20.x)。

## ステップ4: 設定リセット (プロジェクトデータは触らない)

バックアップを必ず先に取ること。

```bash
# バックアップ先作成
mkdir -p ~/Desktop/davinci_recovery

# Project.db バックアップ
cp ~/DaVinci\ Resolve\ Media/<プロジェクト>/Project.db \
   ~/Desktop/davinci_recovery/Project_db_backup_$(date +%Y%m%d_%H%M%S).db

# プロジェクトフォルダ全体バックアップ
cp -R ~/DaVinci\ Resolve\ Media/<プロジェクト> \
   ~/Desktop/davinci_recovery/<プロジェクト>_snapshot_$(date +%Y%m%d_%H%M%S)

# OFXプラグインキャッシュを退避
mv ~/Library/Application\ Support/Blackmagic\ Design/DaVinci\ Resolve/OFXPluginCacheV2.xml \
   ~/Library/Application\ Support/Blackmagic\ Design/DaVinci\ Resolve/OFXPluginCacheV2.xml.DISABLED

# preferences plist を退避
mv ~/Library/Preferences/com.blackmagic-design.DaVinciResolve.plist \
   ~/Library/Preferences/com.blackmagic-design.DaVinciResolve.plist.DISABLED

# plistキャッシュをリセット
killall cfprefsd 2>/dev/null || true
```

DaVinci起動 → 効果なければ元に戻す:
```bash
mv ~/Library/Application\ Support/.../OFXPluginCacheV2.xml.DISABLED \
   ~/Library/Application\ Support/.../OFXPluginCacheV2.xml
mv ~/Library/Preferences/com.blackmagic-design.DaVinciResolve.plist.DISABLED \
   ~/Library/Preferences/com.blackmagic-design.DaVinciResolve.plist
```

## ステップ5: プロジェクトフォルダ退避で起動回避

DaVinciが特定プロジェクトを自動オープンしてクラッシュしている場合:

```bash
# DaVinciを完全終了してから実行
mv ~/DaVinci\ Resolve\ Media/<プロジェクト> ~/DaVinci\ Resolve\ Media/<プロジェクト>_TEMP_HIDDEN

# DaVinciを起動 → Project Manager画面まで到達できたら成功
# その後フォルダを元に戻す
mv ~/DaVinci\ Resolve\ Media/<プロジェクト>_TEMP_HIDDEN ~/DaVinci\ Resolve\ Media/<プロジェクト>

# DaVinci再起動 → Project Manager → Import で手動インポート
```

## ✅ うまく行ったこと

- `PRAGMA integrity_check` + `.tables` + `SELECT COUNT(*) FROM Sm2Timeline` の3点セットでデータ生存を素早く確認できた (2026-05-18実績)
- バックアップ → リネーム → 起動試行の手順は非破壊で安全
- `ResolveDebug.txt` のassertionメッセージからクラッシュ箇所を特定できた

## ❌ 詰まったこと

- 「さなぎ」プロジェクトは Resolve Project Library に登録されていなかった (Blackmagic Cloud同期型フォルダ構成)。`ls Resolve Projects/` に出てこないのでパニックになりがちだが、`~/DaVinci Resolve Media/` 配下に独立したProject.dbとして存在するのが正常
- OFXキャッシュ・plist退避しても `Sm2EffectKeyframe.cpp` クラッシュは止まらなかった。原因がベータ版×macOS深層の場合は設定リセットで解決しない
- `.drb` (自動バックアップ) がゼロの環境があった。DaVinci標準バックアップはOFFになりがち

## 📋 次回同じことをするときのチェックリスト

1. まず `PRAGMA integrity_check` でデータの生死を確認する (「編集やり直し」と早合点しない)
2. `crash_archive.txt` と `ResolveDebug.txt` でクラッシュスタックを取得し、パターンAかBか判別する
3. バックアップ (Project.db + フォルダ全体) を必ず先に取る
4. DaVinciのバージョンが「b」(ベータ) か確認する。ベータ × 最新macOS は不安定なことがある (`DbAppVer` をsqliteで確認できる: `SELECT * FROM CoVersionTable;`)
5. 設定リセットで直らない場合は安定版DaVinciの並列インストールを最短ルートとして検討する
6. `~/Desktop/davinci_recovery/` を作業ディレクトリにすると片付けが楽
