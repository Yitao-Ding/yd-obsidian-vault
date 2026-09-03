---
name: NativeWind v4 Pressable 関数形式 style が握り潰される
description: NativeWind v4 の cssInterop が Pressable の ({ pressed }) => [...] 形式 style を無効化する。PAA UI監査で発見、NativeWind ごと撤去で解決。
type: knowledge
---

# NativeWind v4 Pressable 関数形式 style バグ

## 現象

```tsx
// これが一切効かない
<Pressable style={({ pressed }) => [styles.base, pressed && styles.active]}>
```

押下フィードバックどころか、静的な style 指定も含めて View に届かなくなる。結果として背景色ごと消え、白地に白文字になる。

PAA (project-agent-application) の UI 監査で 45 箇所に影響していた。「アイコン + テキスト行が縦に潰れる」と「CTA が画面に存在しない」の正体がこれだった。

## 根本原因

NativeWind v4 が `cssInterop(Pressable, { className: 'style' })` を jsxImportSource 経由で登録する。この interop が関数形式の `style` prop を理解できず、握り潰す。静的 style に替えた瞬間に正常描画に戻ることで確定。

## 解決策

### 方針1: NativeWind ごと撤去 (PAA で採用)

`className` の使用数が少なく (PAA は 0 件)、Tailwind に依存していなければ、NativeWind を外すのが最も根本的。

`babel.config.js` から NativeWind の jsxImportSource と preset を削除する。

### 方針2: 静的 style に書き換える

関数形式を静的に分解する。ただし `pressed` フィードバックは別手段が必要になる。

## Expo Router (tabs) のレイアウト前提 (同監査で判明)

`(tabs)` 配下の画面はタブバーの**上まで**しかレイアウトされない。`tabBarStyle` に `position: 'absolute'` が無い場合、タブバーは overlay ではない。

- 固定フッターの `bottom` は `0` でよい
- `insets.bottom` を足していた画面はむしろ下に余白が出ていた

## ✅ うまく行ったこと

- NativeWind 撤去後、teams/new の CTA が正常表示されたことをシミュレータで実機確認
- babel.config.js の変更だけで全 45 箇所が一括解決した

## ❌ 詰まったこと

- インラインで `backgroundColor: 'red'` を足しても変わらず、原因特定に時間がかかった
- 静的 style に替えた瞬間に解決したことで根本原因を確定できた

## 📋 次回同じことをするときのチェックリスト

- NativeWind を使うか確認する前に `className=` の使用件数を `grep -rn 'className=' app src | wc -l` で計測する
- 0 件なら NativeWind を外すのが最短
- Pressable に関数形式 style を使うなら NativeWind が入っていないことを確認する
- Expo Router の tabs 配下で固定フッターを作るとき: `bottom: 0` を先に試し、`insets.bottom` の加算は overlay 確認後にする
