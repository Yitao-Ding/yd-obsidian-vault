---
type: knowledge
area: programming
subarea: tools
last_updated: 2026-05-20
status: active
project: Task Hub
related:
  - "[[active_projects]] #6"
  - "[[2026-05-19_TaskHub_git整理_GitHub連携]]"
  - "[[lecture_hub]]"
  - "[[vidkit]]"
---

# Task Hub — 運用マニュアル

## 概要

大学ボランティアサークル「Salamat」(260 名規模) 向けの**モバイルファースト**タスク管理 PWA アプリ。
プロジェクト単位でタスク管理 → メンバー割り当て → 提出物 (テキスト/リンク/ファイル) チャット提出 → プッシュ通知、まで一気通貫。

- **本番 URL**: https://salamat-task-hub.web.app
- **GitHub**: https://github.com/Yitao-Ding/salamat-task-hub (Private、Yitao-Ding 個人 account)
- **デプロイ**: Firebase Hosting (Spark プラン、完全無料)
- **想定ユーザー**: 20〜260 名規模、iPhone Safari PWA メイン (PC レスポンシブも対応)

## 技術スタック

| 項目 | 内容 |
|------|------|
| フレームワーク | Next.js 16.2.1 (App Router、静的 export `output: "export"`) |
| 言語 | TypeScript 5+ |
| スタイル | Tailwind CSS v4 + @tailwindcss/postcss |
| バックエンド | Firebase (Firestore / Auth / Storage)、`firebase ^12.11.0` |
| 管理者操作 | `firebase-admin ^13.7.0` (`scripts/set-admin.mjs`) |
| ホスティング | Firebase Hosting (静的) |
| PWA | `next-pwa ^5.6.0` (Service Worker + manifest) |
| 認証 | Google Sign-In |

### Firebase プロジェクト
- **プロジェクト ID**: `salamat-task-hub`
- **プラン**: Spark (無料)
- App Hosting / Cloud Functions は **使えない** (Spark の制約)

## データ構造 (Firestore)

```
users/{uid}
  - displayName / email / photoURL / role ("admin" | "member") / createdAt
  notifications/{notifId}
    - type / title / body / projectId / taskId / read / createdAt

projects/{projectId}
  - name / description / leaders[] / members[] / status ("active" | "archived")
  - createdBy / createdAt / updatedAt
  tasks/{taskId}
    - title / description / assignee / status ("todo"|"in_progress"|"done")
    - priority ("low"|"medium"|"high") / dueDate / createdBy / createdAt / updatedAt
    submissions/{submissionId}
      - authorId / authorName / authorPhotoURL / type ("text"|"link"|"file")
      - content / fileURL / fileName / fileType / createdAt
```

詳細スキーマ: リポジトリ内 `HANDOVER.md` / `salamat-task-hub-spec.md`。

## 権限モデル (Discord ロール風)

| 操作 | admin | leader | member | assignee |
|------|-------|--------|--------|----------|
| ユーザー管理 | ✅ | - | - | - |
| プロジェクト作成・削除 | ✅ | - | - | - |
| プロジェクト編集 | ✅ | ✅ | - | - |
| タスク作成・削除 | ✅ | ✅ | - | - |
| タスクステータス変更 | ✅ | ✅ | - | ✅ (自分のみ) |
| コメント・提出物追加 | ✅ | ✅ | ✅ (所属PJ) | ✅ |

権限実装は `firestore.rules` で完結 (`isAdmin()` / `isProjectLeader()` / `isProjectMember()` の Firestore custom function)。

## ディレクトリ構成 (要点)

```
src/
├── app/
│   ├── layout.tsx, page.tsx, globals.css     # ルート、PWA meta、AuthProvider 注入
│   ├── login/page.tsx                        # Google ログイン
│   └── (app)/                                # 認証必須ルートグループ
│       ├── layout.tsx                        # 認証チェック + BottomNav
│       ├── dashboard/, projects/, members/, notifications/, settings/
│       └── projects/[projectId]/tasks/[taskId]/  # 動的ルート (二段構成、後述)
├── components/  (BottomNav, ServiceWorkerRegistrar)
├── contexts/    (AuthContext — Google Sign-In + プロフィール upsert)
├── lib/         (firebase.ts 初期化、firestore.ts CRUD)
└── types/       (User/Project/Task/Submission/Notification 型定義)

public/
├── manifest.json, sw.js                      # PWA
└── icons/  (192/512 PNG + SVG, apple-touch-icon)

scripts/set-admin.mjs                         # 初期 Admin 設定 (UID + serviceAccountKey 引数)
firebase.json, .firebaserc, firestore.rules, storage.rules
HANDOVER.md, salamat-task-hub-spec.md
```

## 動的ルートの罠 (静的 export 対応)

`output: "export"` + 動的ルート (`[projectId]`) 構成での **重要な落とし穴**:

- Next.js の静的 export は動的ルートに `generateStaticParams` が必須
- 一方 `"use client"` ページは `generateStaticParams` を持てない
- 解決: **server wrapper + client component の二段構成**

```typescript
// page.tsx (server wrapper)
import ProjectDetailClient from "./ProjectDetailClient";
export function generateStaticParams() {
  return [{ projectId: "_p_" }];  // プレースホルダーを返す
}
export default function ProjectDetailPage() {
  return <ProjectDetailClient />;
}

// ProjectDetailClient.tsx ("use client")
// useParams() で実 URL からパラメータを読む
```

Firebase Hosting 側 (`firebase.json`) の rewrite で `/projects/<real-id>` を `/projects/_p_/index.html` にマップし、クライアント側 `useParams()` が実 URL から `<real-id>` を取得。

## デプロイ手順

```bash
npm run build              # out/ に静的ファイル出力
firebase deploy --only hosting
```

セキュリティルールのデプロイ:
```bash
firebase deploy --only firestore:rules,storage
```

⚠ `HANDOVER.md` に「セキュリティルールはまだ本番にデプロイしてない可能性」とあるので、要確認。

## 環境変数 (`.env.local`)

```env
NEXT_PUBLIC_FIREBASE_API_KEY=...
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=salamat-task-hub.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=salamat-task-hub
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=salamat-task-hub.firebasestorage.app
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=...
NEXT_PUBLIC_FIREBASE_APP_ID=...
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=
NEXT_PUBLIC_FIREBASE_VAPID_KEY=
```

`.env.local` は `.gitignore` で除外済 (`.env*` 行)。

## 機密ファイル管理

| ファイル | 場所 | ignore |
|---|---|---|
| `serviceAccountkey.json` | リポジトリ root | ✅ `.gitignore` で多重保護 (大小文字 + パス変種) |
| `.env.local` | リポジトリ root | ✅ `.gitignore` の `.env*` |
| `.firebase/hosting.b3V0.cache` | `.firebase/` | ✅ `.gitignore` の `.firebase/` |

`serviceAccountkey.json` は **絶対にコミットしない**。Firebase Console → プロジェクト設定 → サービスアカウント で再発行可能。

## 初期 Admin 設定

```bash
node scripts/set-admin.mjs <UID> <serviceAccountKey.json のパス>
```

- UID は Firebase Console → Authentication → Users で確認
- serviceAccountKey は Firebase Console → プロジェクト設定 → サービスアカウント → 「新しい秘密鍵を生成」

## git 履歴 (2026-05-20 時点)

```
9dfcd77 docs: add handover document, spec, and admin setup script
47b6be5 feat: PWA setup (manifest, service worker, icons)
7fa1b65 feat: implement core app (auth, Firestore CRUD, routes, UI)
6d0e341 feat: add Firebase backend configuration
f876634 chore: prepare Next.js + Firebase build configuration
6215be3 Initial commit from Create Next App
```

5 commit に統合した経緯は [[2026-05-19_TaskHub_git整理_GitHub連携]] 参照。

## 既知の懸案 / TODO

- [ ] **メンバー招待フロー仕上げ** — 現在は手動で UID 直書き、招待リンク発行 UI 未実装 (spec の「メンバー管理 (Admin 専用)」セクション参照)
- [ ] **PWA 最終調整** — iOS Safari ホーム画面追加時のスプラッシュ・カラー周辺
- [ ] **商用化検討** — 大学サークル向けフリーミアム、Salamat で実運用 → 外部 β → 広告
- [ ] **Firestore / Storage rules の本番デプロイ確認** — HANDOVER.md に「未確認」とある
- [ ] **next-pwa v5 系の互換性** — Next.js 16+ と古め (v5.6.0)、v6 系 / Serwist 系への移行も検討余地

## ✅ うまく行ったこと

- Firebase Hosting + 静的 export の組み合わせで **Spark プラン (完全無料) で動的アプリを実現** (Firestore リアルタイム subscribe は client-side のみ、サーバーサイドは不要)
- 動的ルート二段構成 (server wrapper + client component) で `output: "export"` の制約を回避
- 寒色系カラーパレット (#2563EB Primary) と Inter + Noto Sans JP の組み合わせがモバイル UI に効果的
- Firestore Security Rules の `isAdmin / isProjectLeader / isProjectMember` 関数化で権限ロジックがルールファイル内で完結 (再利用しやすい)
- 8 週間放置してた untracked を 5 commit に綺麗に分割できた (依存順序が明快だった)

## ❌ 詰まったこと

- **`output: "export"` × 動的ルート × `"use client"`** の三つ巴で初期実装時に詰まる (server wrapper を挟む構成にたどり着くまで試行錯誤、HANDOVER.md に経緯が残る)
- **8 週間ノーコミット**で放置 → untracked 32 / dirty 7 まで膨らみ、最初の git 整理コストが高くなった (本来は Day 1 で GitHub 連携すべき)
- **`serviceAccountkey.json` がリポジトリ root に置かれていた**期間があった (機密) → 監視 CC が 2026-05-19 に `.gitignore` 多重保護を入れて回避、流出はなかった
- **active_projects #6 が長期間「Vercelデプロイ完了」と誤記**されていた (実態は Firebase Hosting)。Vault 記述の経年劣化、本作業時に訂正
- **Spark プランの制約**で App Hosting / Cloud Functions が使えず、サーバーサイド処理 (例: タスク割り当て時のプッシュ通知バックエンド) が現状実装できない (FCM は client から直接でも可能だが、ロバストではない)

## 📋 次回同じことをするときのチェックリスト

新しい類似 PWA / Firebase Hosting プロジェクトを立ち上げるとき:

1. **Day 1 で必ず GitHub 連携**: `gh repo create <user>/<repo> --private --source=. --remote=origin --push` を Create Next App 直後に実行
2. **`.gitignore` の機密保護を Day 1 で完備**:
   - `.env*` (`.env.local` 用)
   - `serviceAccount*.json` + `**/serviceAccount*.json` (Firebase Admin SDK 用、パス変種)
   - `.firebase/` (Firebase Hosting cache)
   - `/out/` (Next.js static export 出力先) — Create Next App では入ってないので追加
3. **動的ルート + 静的 export を予定するなら server wrapper / client component の二段構成を最初から**:
   ```
   app/foo/[id]/
     page.tsx                    # server wrapper (generateStaticParams)
     FooClient.tsx               # 実装 ("use client")
   ```
4. **Firebase Hosting の rewrite を `firebase.json` で動的ルートに対応**:
   ```json
   "rewrites": [
     { "source": "/projects/*/tasks/**", "destination": "/projects/_p_/tasks/_t_/index.html" },
     { "source": "/projects/**", "destination": "/projects/_p_/index.html" },
     { "source": "**", "destination": "/index.html" }
   ]
   ```
5. **Firestore Security Rules は最初から関数化** (`isAdmin / isProjectLeader / isProjectMember`)、ルール変更は `firebase deploy --only firestore:rules,storage` で別途デプロイ
6. **初期 Admin 設定スクリプト** (`scripts/set-admin.mjs`) を `serviceAccountKey` 引数受け取り型で作成 (鍵情報をコード内に持たない)
7. **コミット粒度の標準テンプレ** ([[2026-05-19_TaskHub_git整理_GitHub連携]] 参照):
   - chore (build config / deps)
   - feat (backend config = `firebase.json` / `*.rules`)
   - feat (app core = `src/`)
   - feat (PWA assets)
   - docs (`HANDOVER.md` / spec / scripts)
8. **Spark プランの制約**を最初から認識: App Hosting / Cloud Functions なし、Firestore 読み 50k/日 / 書き 20k/日、Storage 5GB

## 関連

- [[active_projects]] #6 Task Hub
- [[2026-05-19_TaskHub_git整理_GitHub連携]] — git 整理の意思決定
- [[vidkit]] — 類似の Python CLI ツール (同じ Yitao-Ding GitHub 個人 account)
- [[lecture_hub]] — 個人ナレッジハブ (Next.js + Supabase 構成、TaskHub は Firebase)
- `HANDOVER.md` (リポジトリ内) — Manus 引き継ぎ用、ソースコード全文・データ構造・既知の注意事項
- `salamat-task-hub-spec.md` (リポジトリ内) — 元の設計仕様書
