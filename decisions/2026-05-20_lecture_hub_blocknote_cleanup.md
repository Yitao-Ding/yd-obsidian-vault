---
date: 2026-05-20
type: decision
category: technical
tags: [lecture-hub, blocknote, tiptap, cleanup, refactor]
related:
  - "[[2026-05-19_tiptap_migration]]"
  - "[[claude_mistakes]] B-4"
---

# Lecture Hub: BlockNote 残骸クリーンアップ (Phase 3)

## 背景

2026-05-19 の TipTap 全面移行 ([[2026-05-19_tiptap_migration]]) で本番デプロイは完了したが、以下の残骸がコードベースに残置されていた:

- `*.bak` ファイル 5件 (旧 BlockNote ファイルを退避したもの)
- `blocknote-overrides.css` (どこからも import されていない CSS)
- `@blocknote/{ariakit,core,react}` パッケージ (dependencies に残置、-98 packages の廃ライブラリ群)
- `src/lib/blocknote/text.ts` の `plainTextFromDocument` が旧 BlockNote 配列形式のまま (全文検索・pgvector embedding の前提関数なので放置厳禁)
- ディレクトリ名 `src/lib/blocknote/` が TipTap 移行後もそのまま残置

## 実施内容 (4 commits、push なし)

| commit | 内容 |
|--------|------|
| `591368b` | .bak 5件 + blocknote-overrides.css 削除 (-742 lines) |
| `9e45956` | @blocknote/{ariakit,core,react} pnpm remove、-98 packages |
| `234f8ad` | plainTextFromDocument を TipTap 形式に書き換え (vitest 5件全通過) |
| `a158830` | src/lib/blocknote/ → src/lib/editor/ (git mv)、embed-action.ts import パス更新 |

### plainTextFromDocument の変更詳細

旧 (BlockNote 配列形式):
```ts
// 期待: [{ type: "heading", content: [{ type: "text", text: "Title" }] }, ...]
if (!Array.isArray(doc)) return "";
```

新 (TipTap doc 形式):
```ts
// 期待: { type: "doc", content: [{ type: "heading", content: [...] }, ...] }
if (!doc || typeof doc !== "object") return "";
const root = doc as { type?: string; content?: unknown[] };
if (root.type !== "doc" || !Array.isArray(root.content)) return "";
```

再帰 `nodeText` 関数:
- `{type:"text"}` ノードはテキストを直接返す
- `bulletList / orderedList / listItem / blockquote` は子を `"\n"` 区切り
- それ以外 (paragraph / heading / codeBlock 等) は子をインライン結合 (`""`)

### 削除した関数

`plainTextAround(doc, blockId, n)` — `ai-slash-items.tsx.bak` 内でのみ使用。.bak 削除と同時に不要になったため撤去。

## 検証結果

| 確認項目 | 結果 |
|---------|------|
| vitest run text.test.ts | ✅ 5件全通過 |
| pnpm exec tsc --noEmit | ✅ エラーゼロ |
| pnpm build | ✅ 12ページ全生成、エラーゼロ、4.6秒 |
| pnpm why @blocknote/core (事前) | ✅ 直接依存のみ、外部への影響なし |
| git mv による履歴保持 | ✅ renamed: src/lib/{blocknote => editor}/text.ts |

---

## ✅ うまく行ったこと

- **`pnpm why` 事前確認が有効**: `@blocknote/core` は `lecture-hub` 直接依存のみで、外部パッケージが再依存するケースなし → `pnpm remove` を安全に実行できた
- **`git mv` で履歴保持**: `src/lib/blocknote/text.ts → src/lib/editor/text.ts` の rename が git 履歴として残り、`git log --follow` で追跡可能
- **import パスの影響が最小**: 本番コードで `@/lib/blocknote/text` を使っていたのは `embed-action.ts` の 1 ファイルのみ。`.bak` ファイル内の参照は削除と同時に消えた
- **テスト先にリダクト戦略が効いた**: TipTap 形式フィクスチャでテストを書き直してから実装を確定させたため、期待動作が明確だった
- **`sync.ts` が形式非依存**: Dexie のオフライン同期は `document` を opaque な object として扱うだけで、BlockNote/TipTap の構造差に依存しない → 書き換え不要と確認できた

## ❌ 詰まったこと

- **該当なし**: 作業は全て計画通り進行した。特に詰まった箇所はなし
- 強いて言えば、`plainTextAround` の削除判断 (`.bak` ファイル内のみ使用か確認するグレップが必要だった) を最初から計画に含めていなかったが、実際に `grep` したら `.bak` 内のみと判明し問題なかった

## 📋 次回同じことをするときのチェックリスト

### エディタ移行後のクリーンアップ一般

- [ ] `.bak` ファイルを `find . -name "*.bak"` で全列挙してから `rm` (A-7: rm 前確認)
- [ ] 移行後の CSS ファイルを `grep -r` で参照元確認してから削除
- [ ] 削除対象パッケージを `pnpm why <pkg>` で外部依存を事前確認
- [ ] `pnpm remove` 後に `pnpm install` で lockfile を整合化

### plainTextFromDocument 的な「ドキュメント形式依存関数」の移行

- [ ] grep で全 import 箇所を洗い出す (`grep -r "from.*@/lib/..."`)
- [ ] TipTap doc の実際の JSON 構造を確認 (型定義や `console.log` で確認)
- [ ] テストを先にTipTap形式フィクスチャで書き直してから実装する
- [ ] `vitest run` で全通過を確認してから commit

### ディレクトリリネーム

- [ ] `git mv` で履歴保持 (単純な `mv + git add` はリネームではなく削除+新規として扱われる)
- [ ] リネーム後すぐに `grep -r "from.*旧パス"` で全 import を洗い出す
- [ ] `tsc --noEmit` でパス漏れを検出

## 関連

- 親: [[2026-05-19_tiptap_migration]]
- ミスログ: [[claude_mistakes]] B-4 / A-7
- 実装: `~/projects/lecture-hub/src/lib/editor/text.ts`
- 本番: https://lecture-hub-sable.vercel.app/
