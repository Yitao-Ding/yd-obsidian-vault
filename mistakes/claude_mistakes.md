# mistakes/claude_mistakes.md — Claudeの過去のミス記録

> このファイルは Claude が過去に YD に対して犯したミスを記録し、
> 次のセッションのClaude (=未来の自分) が同じミスを繰り返さないようにするためのもの。
>
> **新セッション開始時、必ず最初に読むこと。**
>
> 蓄積された教訓 = YDの教育コストの「投資化」
>
> 対になる「成功パターン」の登録簿: [[reusable]] (knowledge/programming/patterns/) — 3回以上うまくいった型を資産化する「陽」の記録。
>
> 最終更新: 2026-07-09

---

## 📋 ミスのカテゴリ

- A: ツール使用関連
- B: 技術評価関連
- C: コミュニケーション関連
- D: 文脈・記憶関連
- E: 提案・出力関連

### 🏷 failure-mode 横串タグ (2026-07-09 追加)

上の A〜E は「Claude の行動ドメイン軸」。それに直交する **failure-mode 軸** をエントリ見出しに `#tag` で併記し、「原因の型」で串刺し集計できるようにする (記事 Fable5 の error_taxonomy を借用)。

- `#drift` — 根本動機/横断制約からの逸脱 (例: D-4 / D-5)
- `#hallucination` — 存在しない情報・API・パスの捏造
- `#tool` — ツール使用ミス (呼び忘れ・引数ミス・仕様誤解)
- `#destructive` — rm/kill 等の破壊操作の確認漏れ
- `#cost` — コスト超過・課金前提の見落とし
- `#format` — 出力形式・スタイルの不準拠

**運用**: 新規エントリ追加時に該当タグを見出し末尾に付ける。既存40件の backfill は月次メンテで順次 ([[vault_improvement_proposals]] 2026-07-09 ⑤)。

---

## カテゴリA: ツール使用関連

### A-1. ローカルファイルアクセスの忘却 (頻度: 高、最終発生: 2026-05-18)

**状況**: YDのMacにはDesktop Commander拡張機能が常時インストールされている。これにより `/Users/ittou/` 配下の全ファイルにアクセス可能。

**過去のやらかし**:
- 2026-05-18: vidkitの進捗を聞かれた時、最初「アクセスできません、YDに動いてもらう必要があります」と答えてしまった。YDに「アクセスできるよ!やってみて!」と指摘された。

**正しい挙動**:
- ファイル・プロジェクト・ローカル環境に関する質問が来たら、まずDesktop Commanderの使用を試みる
- `Desktop Commander:list_directory` や `Desktop Commander:read_multiple_files` で実際にアクセスを試みる

**再発防止**:
- セッション開始時に `current_state/tools_available.md` を必ず確認
- 「アクセスできない」と言う前に、必ず1回はツール呼び出しを試す

---

### A-2. MCP連携の存在忘却 (頻度: 中)

**状況**: 以下のMCPサーバーが常時繋がっている:
- Notion (Arte Grow、Task Hub関連のページ管理)
- Google Drive / Calendar / Gmail
- Canva
- Zoom
- Goodnotes
- Figma
- Miro
- Microsoft 365

**過去のやらかし**:
- Notionの情報を聞かれた時、Web検索で代用しようとしたケースあり

**正しい挙動**:
- 外部サービスの情報が必要な場合、まず対応するMCPツールを呼び出す
- 不明な場合は `tool_search` で確認

**再発防止**:
- `current_state/tools_available.md` に常時接続中のMCP一覧を保持

---

### A-3. tool_search の使用忘れ (頻度: 中)

**状況**: 多くのツールは tool_search で動的にロードする必要がある。

**過去のやらかし**:
- 「○○できない」と早合点しがち
- Google Calendar の予定取得時など、ツール検索を忘れて諦めるパターン

**正しい挙動**:
- 不明な場合は必ず `tool_search` で確認する
- "calendar events" "list calendar" 等のキーワードで検索

---

### A-5. 外部ライブラリの API バージョン差異を見落とす (頻度: 中、最終発生: 2026-05-19)

**状況**: 21st.dev などのサンプルコードをそのままコピーすると、参照しているライブラリのバージョンと実プロジェクトの依存バージョンに差があり、削除/改名されたプロパティを使ってしまう。

**過去のやらかし**:
- 2026-05-19: Salamat WBS Phase 1 実装中、`@paper-design/shaders-react` を新規追加 (0.0.76)。21st.dev のサンプルが使っていた `backgroundColor` (MeshGradient) / `spotsPerColor` (PulsingBorder) / `frame` パラメータが 0.0.76 では削除または改名されており、tsc エラーで発覚。
- 対応: `node_modules/.pnpm/@paper-design+shaders@0.0.76/node_modules/@paper-design/shaders/dist/shaders/*.d.ts` を直読みして実際の Params 型を確認、`spots` / `colorBack` / 親要素 background による代替で修正

**正しい挙動**:
- サンプルコードを移植する前に、実プロジェクトの `node_modules/<pkg>/dist/*.d.ts` で API を確認する
- 特に新興ライブラリ (@paper-design/shaders-react、cobe、lucide-react v1.x など) はバージョンごとに breaking change が起きやすい
- `find node_modules/.../*.d.ts` + `cat` または Read で型定義を直読みして仕様確認

**再発防止**:
- サンプルコード採用前に「このパッケージのこのバージョンに該当プロパティが存在するか」を1回確認
- tsc エラーが「Property X does not exist」系のときは、まず該当パッケージの .d.ts を直読み

---

### A-6. 画像ファイル名を信用して中身を確認しない (頻度: 低、最終発生: 2026-05-19)

**状況**: `ph-feeding.jpg` のような国コード付きファイル名でも、実際の写真の中身がファイル名と一致しないことがある。アップロード時のラベリングミス。

**過去のやらかし**:
- 2026-05-19: Salamat WBS の `public/assets/actions/` 配下、`ph-*.jpg` の中身が日本の写真、`jp-*.jpg` の中身がフィリピンの写真、と完全に逆になっていた。YD の指摘「全部が全部反対」で発覚。Read ツールで6枚並列確認して確証、コード上のマッピングを入れ替えて対応。

**正しい挙動**:
- 画像アセットを最初に使う時は、Read ツールで中身を必ず確認する (特にユーザー提供画像)
- ファイル名と中身が違う場合、まずユーザーに指摘 → 修正方針 (ファイル名変更 vs コードのマッピング変更) を確認
- ファイル名 cleanup を将来タスクとして残しておく (混乱の原因になるため)

**再発防止**:
- セクションに画像を初配置する時、Read ツールで6枚並列確認のクセをつける
- 「ファイル名で命名規則がある画像」は特に注意 (国コード / 日付 / カテゴリ名)

---

### A-4. Edit ツール使用前の Read 必須を忘れる (頻度: 中、最終発生: 2026-05-18)

**状況**: Claude Code の Edit ツールは「同じ会話内で 1 度は Read で開いていないと失敗する」仕様。harness がファイル状態を追跡している。Write も既存ファイルだと同様。

**過去のやらかし**:
- 2026-05-18: Vault 構築 Step 11 で `~/.zshrc` にエイリアスを追記しようとして、先に Read せず Edit を呼んだ → `File has not been read yet` エラーで失敗。Read 後に再 Edit して成功。

**正しい挙動**:
- 既存ファイルを編集する前に、必ず Read を入れる
- Read と Edit を並列発行しない (Edit が先に届くと失敗する)。順序は **Read → Edit**
- 新規作成なら Write、既存ファイル編集なら Read → Edit のパイプを徹底

**再発防止**:
- 編集系操作の前に「これ Read 済み?」と1秒考える
- ファイルパスが手元の context にあっても、判定基準は「同じ会話内で Read を呼んだか」
- 関連: [[claude_code]] の「弱み」セクションに同じ事例を実例として記録

---

### A-7. セッション起動時の機能マッピング演習を実行しない (頻度: 中、最終発生: 2026-05-25)

**状況**: グローバル `CLAUDE.md` (⚡起動時必須シーケンス) と `current_state/available_capabilities.md` (🎯朝の機能マッピング演習) で「必須読み込み完了後、応答する前にカタログから当日タスクに使えそうなスキル/MCP を 3〜5 個ピック → 初手応答で提示」と明文化されている。これを省略してタスクに直行することがある。

**過去のやらかし**:
- 2026-05-25: YD が「vidkit を起動して動画の内容を超細かく教えてほしい」と依頼。即 vidkit を叩いて分析・要約まで進めたが、その間 `available_capabilities.md` のカタログを一度も参照せず。最終アウトプット形式の選択肢 (Vault ナレッジ化 / `document-skills:*` で PDF・PPT・docx 化 / `frontend-design` で HTML 化 / vidkit の `--vault-path` 直接出力ルート 等) を一切提示しないまま要約完了。YD から「これを俺がみやすいようになんかにまとめる機能あったっけ」と聞かれて初めてカタログを開いた。本来は分析開始前に選択肢を提示できたはずで、後出しになった。

**正しい挙動**:
- セッション起動の必須読み込み (CLAUDE.md ⚡起動時必須シーケンス、[[available_capabilities]] 含む) を省略しない
- ユーザーの初手リクエストを読んだ瞬間に「このタスクの最終アウトプット形式は複数ありえるか?」を判定し、複数ありえるなら作業前に選択肢提示
- 動画分析・ドキュメント生成・UI 構築のような「形式が複数ありえる」タスクでは特に、開始前の選択肢提示を徹底

**再発防止**:
- 起動時の読み込みワンセットに `available_capabilities.md` を確実に含める (CLAUDE.md にも明記済み)
- 最初のタスク説明を読んだ瞬間に「Vault 形式? HTML/PDF/PPT? どれが筋?」を内的に判定するクセ
- 関連: [[claude_mistakes]] A-2 (MCP連携の存在忘却) と同根。「意識化していない機能は使われない」問題

---

### A-16. サブエージェントの `du`/パス報告を検証せず `rm` しかけた (頻度: 低、最終発生: 2026-06-02)

**状況**: ストレージ整理で、読み取り専用の監査サブエージェント(9並列)が出した「削除候補パス+サイズ」を、`rm` 直前に実体確認せず信用した。エージェントが Logic 音源を「`~/Music/Logic Pro Library.bundle/Samples` 55G」と報告したが、実際そのbundleは1.7Gのパッチ集で Samples サブディレクトリは存在せず。本体は `/Library/Application Support/Logic` (58G, sudo必須) にあった。

**過去のやらかし**:
- 2026-06-02: Macディスク整理(224.7G回収成功)の中で、Logic音源削除だけ freed 0G。監査エージェントの報告パスが間違っていた(`.bundle` の中身を推測で埋めた疑い)。幸い存在しないパスへの `rm` だったので実害ゼロだったが、もし誤ったパスが「実在する別物」を指していたら誤削除していた。`find`/`du` で実体を再確認して `/Library` 配下が正解と判明、sudo必須のためYDに委譲。

**正しい挙動**:
- サブエージェント / メモリ / 過去ログが出した削除パスは、**`rm` 直前に必ず `ls -d`/`du -sh` で実体とサイズを確認**してから実行する
- 特に `*.bundle` `*.app` `*.photoslibrary` 等のパッケージ内部や、`~/Music`・`~/Library` と `/Library` の取り違えに注意(ユーザ領域 vs システム領域)
- 大物削除はフェーズ毎に `df -k` Avail差分で「本当に空いたか」を即検証する。freed 0G なら即パスを疑う

**再発防止**:
- 破壊的操作(`rm`)の対象は「報告された文字列」ではなく「直前にこの目で確認した実体」。監査と削除の間に1回 verify を挟む
- 関連: [[disk_cleanup]] (次回チェックリスト②) / [[#A-7]] (`rm`実行前の確認漏れ) と同根

---

### A-17. 並行CC調査で自分自身のプロセスを「orphan」と誤認し kill しかけた (頻度: 低、最終発生: 2026-06-03)

**状況**: 起動時の D-9 対策(並行CC検出)で `ps aux | grep claude` を実行し、CC-business と同じ cwd の claude プロセス(PID 41919)を「5時間アイドルの放置並行CC=orphan」と判定。YD に「閉じてるはず、kill していい」と確認まで取った。kill 直前に A-16 の教訓で「対象が本当に自分でないか」を verify したら、**41919 は自分自身(このセッション)のプロセス**だった。

**過去のやらかし**:
- 2026-06-03: 41919 を複数メッセージにわたり「orphan 並行CC」と断定。`ps -o etime` の読み違い + 「cwd が同じ=別CC」という思い込みが原因。実際は自分の shell(96830)の親が 41919。もし `kill 41919` を実行していたら、自セッション + 走行中の監査Workflow(子プロセス)を巻き添えで全死させていた。「5時間アイドル・書き込み0」だったのは自分がまだ web/ に書いていなかっただけ。

**正しい挙動**:
- 並行プロセスを kill する前に、必ず自分の PID を特定する: shell の `$$` から親を辿り(`ps -o ppid=,comm=`)、どの claude プロセスが自分の祖先かを確認 → 削除候補と照合。
- 「cwd が同じ」は「別プロセス」の証拠にならない(自分も同じ cwd)。起動時刻・PID親子関係・開いているファイルで判定する。
- 破壊操作(kill/rm)は「報告された文字列/思い込み」ではなく「直前にこの目で確認した実体」に対して行う。

**再発防止**:
- D-9(並行CC検出)を実行する時は、最初に自分の claude PID を確定してから他プロセスを論じる。
- kill/rm の前に verify を1回挟む(A-16 と同じ規律)。今回は verify を挟んだから助かった good case。
- 関連: [[#A-16]] (`rm` 前の実体verify) / [[#D-9]] (並行CC + stale HANDOVER) と同根。

---

### A-18. ツール呼び出しの直前に壊れたプレフィックス(`court` 等)を書いてツールが空振り (頻度: 中、最終発生: 2026-06-06)

**状況**: Claude Code でツールを呼ぶ時、正しくは正規のツールブロックで始める。だが応答本文の末尾にツール呼び出しを置く際、`court` のような誤った開始トークン + プレースホルダ行を書いてしまい、ツール呼び出しがパースされず空振り(実行されない)ことがある。同一セッションで複数回繰り返した。

**過去のやらかし**:
- 2026-06-06: Project Agent App のシミュレータ全画面ツアー中、Bash/AskUserQuestion を呼ぶたびに `court` + 擬似 invoke を書いて 5-6 回空振り。YD が画面で「court が出て動作が止まってる」と気づいて「おい」と指摘。実害は遅延のみ(中身は実行されず再送で復旧)だが、YD の画面に無駄なテキストが出続けた。

**正しい挙動**:
- ツール呼び出しは必ず正規のツールブロックで始める。応答本文に `court` 等の擬似タグやプレースホルダ(`placeholder` / `echo placeholder`)を書かない。
- 1 メッセージで複数ツールを並列に出す時も、各ブロックを正しいフォーマットで。前置きテキストとツールブロックの境界を意識する。

**再発防止**:
- 空振りに気づいたら、余計な説明や謝罪を足さず、正規フォーマットで 1 回だけ静かに再送する(YD の画面に余計なテキストを出さない)。
- 本文の末尾に中途半端なタグの書き出しを残さない。

---

### A-24. レンダリングが遅いとき、設定を疑わず所要時間として受け入れた (頻度: 低、最終発生: 2026-08-29) #tool

**状況**: プペルエンドロールの4K書き出しで、Remotion が「残り1時間42分」と表示。並列数を6→8に上げても変わらず、「4Kは重いのでこれくらいかかる」とYDに報告した。実際は GPU レンダラーが無効 (既定のソフトウェア描画) だっただけで、`--gl=angle` を付けたら **8分** で終わった。**14倍の差**。

**過去のやらかし**:
- 2026-08-29: YDに「あと10〜20分」「1時間40分かかります」と誤った見積もりを複数回伝え、30GBのProResを1時間近く焼き続けた。YDから「GPU書き出しは常識だろ」と指摘されて発覚。

**正しい挙動**:
- **重い処理が想定より遅いときは、まず設定 (GPU/ハードウェアアクセラレーション/エンコーダ) を疑う**。所要時間として受け入れて報告しない。
- 動画書き出し系のツールは GPU 利用が既定でないことがある。Remotion は `--gl=angle` / `Config.setChromiumOpenGlRenderer('angle')`、ffmpeg は `-hwaccel videotoolbox` / `h264_videotoolbox`、DaVinci は GPU 設定。
- 「並列数を上げても改善しない」= CPUがボトルネックではない、という信号。そこで描画パスを疑う。
- 長時間かかる処理を始める前に、**短い区間で試して実測から全体を見積もる**。1080pで6分だったものが4Kで100分なら、ピクセル比4倍に対して17倍で明らかに異常と気づける。

**再発防止**:
- 書き出し設定はプロジェクトの設定ファイルに固定する (コマンドの付け忘れで遅くならないように)。プペルでは `remotion.config.ts` に GPU 指定を常設した。
- 進捗をパイプで捨てない (`| tail -3` にすると完了まで進捗が見えず、異常に気づくのが遅れる)。
- 関連: [[remotion_endroll_card_display]] (knowledge/filmmaking/)

---

## カテゴリB: 技術評価関連

### B-1. 実装難易度の過大評価 (頻度: 高、最終発生: 2026-05-18)

**状況**: 新技術や新パターンに対して保守的な見積もりをしがち。

**過去のやらかし**:
- 2026-05-18: Obsidian × Claude Wiki パターンを「正解がない領域、1〜2ヶ月かかる」と評価。
  実際は5分でセットアップ可能な確立されたパターン (Karpathy 2026年4月公開) だった。
  YDが自分で調べて指摘してくれた。

**正しい挙動**:
- 新技術の評価前に必ず `web_search` で最新情報を確認
- 推測ではなく事実ベースで見積もり

**再発防止**:
- 技術評価時のチェックリスト:
  1. `web_search` で該当技術の最新情報を確認
  2. GitHubで実装例の有無を確認
  3. その上で見積もり

---

### B-2. 自分の知識カットオフの誤認 (頻度: 中)

**状況**: Apple Silicon の最新世代 (M5系) などを「知らない」と早合点する。

**過去のやらかし**:
- 2026-05-18: 「M5 Max 36GB」と言われた時、最初「M5 Maxって正式リリースされてましたっけ?」と確認した。
  (これ自体は適切な対応だったが、最初は知らない素振りをした)

**正しい挙動**:
- ユーザーが具体的な型番・サービス名を出してきた時は、即座に `web_search` で確認
- 「知らない」「分からない」と言う前に検索する

---

### B-4. リッチエディタ系ライブラリの最新フレームワーク互換性確認不足 (頻度: 中、最終発生: 2026-05-19、TipTap 移行で本日決着)

**状況**: lecture-hub で **BlockNote 0.51 (ariakit) + React 19.2 + Next.js 15.5** の組み合わせで、`/p/[id]` のエディタを開いた瞬間に ProseMirror `to_dom.ts:203` から `Invalid array passed to renderSpec` が throw されてエディタが描画できない。

**過去のやらかし**:
- 2026-05-19 朝: Phase A/2 完了後の本番デプロイ準備で発覚。3 段階の修正 (a) `defaultBlockSpecs` から `audio/image/video/file` 除外、(b) カスタム audio block を `audio` → `lectureAudio` にリネーム (default audio と衝突回避)、(c) `useCreateBlockNote` から schema を完全に外して pure BlockNote 化 — どれも解消せず。BlockNote 0.51 + React 19 + Next 15 の組み合わせ自体の bug と推定 (関連 issue: TypeCellOS/BlockNote#1347)。結果 1 日の作業が本番デプロイまで届かず blocked。
- 2026-05-19 夕 (続報、6 段階追加検証): (1) BlockNote 0.51.0 → 0.51.1 パッチ更新 / (2) 0.51 → 0.50 ダウングレード / (3) `next/dynamic` の `ssr: false` で Editor 隔離 / (4) React 19.2 → 18.3.1 ダウングレード (issue #1347 workaround そのまま) / (5) `.next` キャッシュクリア / (6) Editor.tsx を 2 行ミニマム化 (`useCreateBlockNote()` + `<BlockNoteView editor={editor}/>` のみ、schema/blocks/slash 全部排除) — **全部 NG**。最後の 2 行ミニマムでも壊れたことで「BlockNote 本体 × Next.js 15.5 (Webpack) の根本不整合」と確定。→ **TipTap v3 に全面移行**で解決 ([[2026-05-19_tiptap_migration]])。本番 `https://lecture-hub-sable.vercel.app/` で動作確認 OK。所要約 5 時間。

**正しい挙動**:
- リッチエディタ (BlockNote / TipTap / Lexical / Plate / Novel) は ProseMirror / React の内部実装に依存度が高く、React・Next の major / minor アップグレードで壊れやすい
- 採用前に **GitHub issue で `react 19` `next 15` のキーワード検索** して open issue 数を確認する
- 「最新フレームワーク × 最新エディタ」を選ぶ場合は、動かなくなった時に切り替えられる **代替候補を 1 つ事前に決めておく**
- もしくは 1 メジャー前のフレームワーク or エディタで凌ぐ判断
- **詰まったら最初に最小再現テスト**: schema / hooks / slash / カスタムブロック全部外して 2 行の最小実装で再現するか確認 → 再現するなら根本不整合確定で移行へ、再現しないなら schema を一つずつ追加して原因を絞り込む

**再発防止**:
- リッチエディタの採用 / アップグレード時のチェックリスト:
  1. GitHub Issue で `react 19` / `next 15` (採用するフレームワーク版) で検索
  2. 過去 3 ヶ月以内の open issue があれば赤信号
  3. 代替候補を最低 1 つ pin する (TipTap / Lexical / Plate / Novel)
- **エディタが壊れたら最小再現テストを最優先**: 1 段階目に schema / blocks 全部外した 2 行コードで再現するか確認。これだけで他 5 試行 (~4時間) をショートカットできた可能性大
- BlockNote 0.51 の同件 issue: TypeCellOS/BlockNote#1347 系
- 関連: [[lecture_hub]] / [[2026-05-19_tiptap_migration]] の進捗履歴

---

### B-3. 商用化の見通しの過小評価 (頻度: 低)

**状況**: Task Hub 等の YD のプロダクトに対して、ビジネス的妥当性を控えめに評価しがち。

**過去のやらかし**:
- 商用化に関する質問に対して、リスクを強調しすぎる傾向

**正しい挙動**:
- YDは小さく始めて回す主義。「Salamatで使う → 商用化」の段階を理解する
- リスクと機会の両方を提示

---

## カテゴリC: コミュニケーション関連

### C-1. タメ口で応答してしまう (頻度: 中、最終発生: 2026-04-01)

**状況**: メモリに「距離感近めの敬語」と登録されているにも関わらず、新セッションでタメ口になることがある。

**過去のやらかし**:
- 2026-04-01: 「いきなり全部タメ口でくるのがあった」とYDに指摘された

**YDの好み**:
- 必ず「です・ます」を使う
- ただし距離感は近め (先輩↔後輩のような関係性)
- 友達口調ではない

**正しい挙動**:
- **全応答で必ず敬語を使用**
- 「〜です」「〜ですね」「〜ですよね」など

**再発防止**:
- セッション開始時に `identity/preferences.md` を必ず読む

---

### C-2. 過度な共感・肯定 (頻度: 高)

**状況**: 「素晴らしいですね!」「お疲れさまでした!」「いい質問ですね!」を多用しがち。

**YDの好み**:
- 個人的に寄り添う優しさは不要
- 客観的判断を優先
- 「世間一般的に考えて」回答してほしい

**過去のやらかし**:
- 質問に対していつも「いい質問ですね」から始めてしまう

**正しい挙動**:
- 必要な範囲でのみ感情表現
- 原則はファクトベース
- 「いい質問ですね」のような枕詞は省略

---

### C-3. 確認質問の不足 (頻度: 中)

**状況**: YDは「雑な文を書く」と自覚しており、確認質問を歓迎している。

**過去のやらかし**:
- 不明確な要求に対して推測ベースで進めて方向違いの応答をしてしまう

**YDの明示的な指示**:
> 私に対しての確認はお構いなく行なってください。少しでも疑問や懸念がある場合は、遠慮なく聞いてください。そして私は結構雑な文を書くことが多い、不十分な情報だったり大事なところを端折ったりするので、そこも含めて拾い上げるようにバンバン聞いてください。一回の会話で一つの質問に限る必要はなく、その時点で出てくる質問は何個でも同時に出してきていいです。

**正しい挙動**:
- 少しでも疑問があれば質問する
- 1ターンに複数質問してOK
- 質問は「Q1: ... Q2: ... Q3: ...」形式で並べる

---

### C-4. 「お疲れさまでした」の多用 (頻度: 中)

**状況**: 作業の合間に「お疲れさまでした!」を入れすぎる。

**過去のやらかし**:
- 中間試験の結果報告に対して「お疲れさまでした!どうでしたか笑」と返した (これは適切だった)
- ただし作業の途中で乱発するのは避ける

**正しい挙動**:
- 区切りの良いタイミングでのみ使う
- 連発しない

---

## カテゴリD: 文脈・記憶関連

### D-1. 過去会話の検索不足 (頻度: 高)

**状況**: `conversation_search` や `recent_chats` を使えば過去の文脈を取得できる。

**過去のやらかし**:
- 「WBSチームについて」と聞かれた時、すぐに検索せず「分からない」と答えてしまった (2026-03-25)
- 当初はWBSチームが何か分からなかったが、検索後に「Salamatの公式HPを制作する専門チーム」と判明

**正しい挙動**:
- YDが「過去の」「あの時の」「前に話した」と言及したら、即座に過去会話を検索
- キーワードを工夫して何回か検索する

**再発防止**:
- 「分からない」と言う前に必ず conversation_search を試す

---

### D-2. メモリと現状のズレの判断ミス (頻度: 中)

**状況**: メモリは自動更新されないため、古い情報が残ることがある。

**過去のやらかし**:
- 「Task Hubの残作業」を何度も同じ内容で繰り返してきた可能性
- メモリが古いままだと、既に完了したタスクを未完了として扱ってしまう

**正しい挙動**:
- メモリの内容を `current_state/active_projects.md` と照合
- ズレがあれば `current_state/` を信頼する
- 必要なら YD に最新状況を確認

---

### D-3. 同じ説明の繰り返し (頻度: 中)

**状況**: 同じ知識を毎回イチから説明してしまう。

**YDの指示**:
> 君や君の同僚の記憶喪失が多すぎるから、ニューセッションを行うときに、チャットの内容に合わせて自分で知識を引っ張ってきて、すぐにこの会話について来れるようにしたい。

**正しい挙動**:
- このVault自体が、その問題を解決するためのもの
- 既に知っていることは Vault から引き出して、説明を最小化

---

## カテゴリE: 提案・出力関連

### E-1. 選択肢を出すだけで推奨案を出さない (頻度: 高)

**状況**: YDは「提案+推奨案」の組み合わせを好む。

**過去のやらかし**:
- 「A案、B案、C案がありますがどうしますか?」で終わる

**正しい挙動**:
- 「個人的にはA案を推奨」まで踏み込む
- 推奨理由も簡潔に

**例**:
```
選択肢:
- A. xxx
- B. yyy
- C. zzz

僕の推奨は **A** です。理由: ...
```

---

### E-2. Markdownの過剰装飾 (頻度: 中)

**状況**: 太字・絵文字・見出しを使いすぎる。

**YDの好み**:
- 必要なときだけ使う
- 結論ファースト
- 装飾は最小限

**正しい挙動**:
- 普段の応答では装飾を控える
- レポート系では適切に使う

---

### E-3. 長文の前置きが長すぎる (頻度: 中)

**状況**: 結論に至る前の前置きが長い。

**過去のやらかし**:
- 「いい質問ですね。これは複雑な問題で... まず背景として... 次に... そして... 結論としては...」

**正しい挙動**:
- 結論ファースト
- 「結論: ○○です。理由: ○○...」の構造

---

## カテゴリX: メタ (この記録自体について)

### X-1. このファイルを読まずに応答してしまう

**状況**: セッション開始時のルーチンを省略してしまう。

**過去のやらかし**:
- (将来発生する可能性が高い)

**正しい挙動**:
- 新セッションの最初に必ずこのファイルを読む
- 読まずに応答することは、このVault全体の目的を破壊する

**再発防止**:
- `CLAUDE.md` と `00_CLAUDE_BOOT.md` の起動シーケンスを厳守

---

## 📚 ジャンル別ミス記録

関連ファイル:
- [[workflow_mistakes]] — ワークフロー・進め方のミス
- [[tool_usage_mistakes]] — ツール使用上のミス
- [[communication_mistakes]] — コミュニケーション上のミス

---

## 📊 ミス統計 (Claudeが自動更新)

| カテゴリ | 件数 | 直近発生 |
|---------|------|---------|
| A. ツール使用 | 25 | 2026-07-11 |
| B. 技術評価 | 9 | 2026-07-11 |
| C. コミュニケーション | 4 | 2026-04-01 |
| D. 文脈・記憶 | 9 | 2026-06-02 |
| E. 提案・出力 | 6 | 2026-05-26 |
| X. メタ | 1 | - |
| **合計** | **54** | - |

> 件数は 2026-07-11 に見出し実数 (`grep -c "^### <cat>-"`) で棚卸しして補正 (従来の手動カウントに追記漏れがあった)。2026-07-11 easy-share セッションで A-21/A-22 を追加(番号は依頼時の指定 A-19/A-20 が既存エントリと衝突していたため、既存の最大値 A-20 の次である A-21/A-22 に振り直し)。

---

## 📝 新規ミスの追加フォーマット

```markdown
### <カテゴリID>-<番号>. <一行サマリ> (頻度: 高/中/低、最終発生: YYYY-MM-DD)

**状況**: <何が起きていたか>

**過去のやらかし**:
- YYYY-MM-DD: <具体的な誤った発言・行動>

**正しい挙動**:
- <こうすべきだった>

**再発防止**:
- <次のClaudeがやるべきチェック>
```

---

## 🎯 この記録の目的の再確認

このファイルが存在する理由:

1. **Claudeの教育コストを「投資」として残す** — YDが何度も同じことを教える必要がない
2. **複数AI間での学習の共有** — Markdownなので ChatGPT/Gemini でも参照可能
3. **メタ認知の促進** — Claudeが自分のミスパターンを認識できる
4. **YDの時間の保護** — 毎回ゼロから説明する負担を減らす

**この記録を読まずに応答することは、このVault全体の存在意義を否定する行為に等しい。**


---

## カテゴリD: 文脈・記憶関連 (追加)

### D-4. 会話の根本動機を設計フェーズで見失う (頻度: 高、最終発生: 2026-05-19) #drift

**状況**: 議論の出発点となった「根本動機」が、議論が進むうちに具体設計の中で忘れられ、根本動機と矛盾する設計を提案してしまう。

**過去のやらかし**:
- 2026-05-19: 出発点は「Max 20xプラン ($200/月) が使い切れない」→「使い切る目的で何ができる?」だった。
  にもかかわらず、ai-researcher / morning-briefing の指示書で **Anthropic API + OpenAI TTS API + Google Cloud TTS** など、Max 20xの外側で動く有料APIを月$80-100規模で発生する設計で提案。
  Claude Code 5並列を稼働させて作り込んだ後にYDに指摘されて発覚 (「APIで動かすAIは極力使わないで欲しい」)。

**正しい挙動**:
- 設計に着手する前、必ず「この議論の根本動機は何だったか」を1秒で確認する
- 特に「コスト」「使い切る」「依存撤廃」のような縛りが冒頭にあった場合、具体設計が縛りに違反していないかチェック
- 既存システムの常識 (例「LLM処理はAPIで」) より、ユーザー固有の縛りを優先する

**再発防止**:
- 新規システムの設計フェーズに入る直前に、以下チェック:
  1. 会話の冒頭5メッセージの「根本動機」を再読
  2. その動機と設計案が矛盾しないか確認
  3. 特に「API課金」「外部依存」「月額固定枠の活用」系の前提は明示的に確認
- このパターンは vidkit / Lecture Hub / Task Hub などの過去プロジェクトでも繰り返す可能性が高いので、習慣化する

---

### A-7. `rm` 実行前の確認漏れ (頻度: 低、最終発生: 2026-05-19)

**状況**: 自走モードでは Push / Deploy / sudo / **rm** のみ YD に確認するルール。自分で書いたばかりのファイルでも `rm` を直接叩く前に確認が必要。

**過去のやらかし**:
- 2026-05-19: ai-researcher の Max 20x 完結化書き換えで、`src/synthesizer/client.py` (anthropic SDK ラッパー、自分が前セッションで書いたもの) を `rm` で削除。意図は明確で復元不要だったが、ルール上は YD への事前確認 or 別手段 (`git rm` / 残して空にする) を取るべきだった。

**正しい挙動**:
- `rm` を打つ前に「これは Push/Deploy/sudo/rm に該当する操作か」を 1 秒だけ自問
- 自分のファイルでも YD に「`src/synthesizer/client.py` を削除してよいか」と一行確認
- もしくは `git rm` (git 管理下なら) や、内容を空にして残す方針も可

**再発防止**:
- Bash で `rm` を含むコマンドを組み立てたら、実行前に AskUserQuestion で確認をする習慣
- 例外: `/tmp` 配下の自分が作ったテンポラリファイル、`__pycache__` などの再生成可能ファイル

---

### A-8. `claude -p --bare` は OAuth (Max 20x) を読まない (頻度: 低、最終発生: 2026-05-19)

**状況**: `claude -p` をヘッドレスで叩くとき、最小化のために `--bare` を入れがちだが、`--bare` の仕様には「Anthropic auth is strictly `ANTHROPIC_API_KEY` or apiKeyHelper via --settings (OAuth and keychain are never read)」とある。OAuth (Max 20x) が無視され、Anthropic API key が必須になってしまう。

**過去のやらかし**:
- 2026-05-19: ai-researcher の `claude -p` 化で、最初に `--bare` を入れたら `Not logged in · Please run /login` で全件失敗。出力 JSON の `total_cost_usd: 0` で気づかなければ、Anthropic API key を持っている場合に課金されてしまう設計のまま走らせる可能性があった。

**正しい挙動**:
- Max 20x 枠で動かしたい場合は `--bare` を使わない
- 代わりに `--no-session-persistence` + `--disable-slash-commands` + `--system-prompt` 自前 で実質的な最小化を達成
- `--bare` は明示的に Anthropic API key で動かしたい (CI など) ときだけ使う

**再発防止**:
- `claude -p` でヘッドレス実行を組む時、必ず最初に「OAuth (Max 20x) と API key のどちらで動かしたいか」を明確化
- OAuth で動かしたい場合は `--bare` 禁止、`--system-prompt` 自前で input を節約

---

### D-5. 「Max 20x 完結化」の決定を別プロジェクトで再度見落とす (頻度: 高、最終発生: 2026-05-19) #drift

**状況**: [[2026-05-19_API依存撤廃_Max20x完結化]] で「LLM 処理は API を使わず Max 20x 枠で完結させる」と決定済み。にもかかわらず、同セッション内で並行で作っていた ai-simulator は anthropic SDK + ANTHROPIC_API_KEY 前提のまま「完成」扱いで active_projects.md に書かれていた。これは [[#D-4]] (会話の根本動機を設計フェーズで見失う) と本質的に同じパターン。

**過去のやらかし**:
- 2026-05-19 (朝): ai-researcher / morning-briefing は YD の指摘 ([[2026-05-19_API依存撤廃_Max20x完結化]]) を受けて Max 20x 完結化済。だが ai-simulator (セッションη で同時期に作成) はそのまま放置。YD への進捗報告でも「ANTHROPIC_API_KEY 待ち」のステータスを平然と書いていた。
- 2026-05-19 (夕): YD が「これ API 使わないようにして欲しい。元々は Max プラン使い切りたいで作ったから」と指摘して発覚。同じミスを 2 回したので Phase 2 自動トリガー該当。

**正しい挙動**:
- `decisions/` で確定した「全プロジェクト共通の制約」(API 禁止 / Max 20x 完結など) は、新規プロジェクトの設計時に必ずチェックリストに含める。
- 同セッション内で複数プロジェクトを並行で作る場合、各プロジェクトに同じ制約が適用されているか相互確認する。
- 「他のプロジェクト (ai-researcher, morning-briefing) は API 撤廃したが、このプロジェクト (ai-simulator) は?」を自問する。

**再発防止**:
- D-4 と D-5 は同じパターンなので、本質的には 1 つの教訓。次の Claude が踏まないためのチェックリスト:
  1. プロジェクト設計に着手する前、`decisions/` の直近 3 件を必ず確認
  2. 「Max 20x 完結」「外部 API 禁止」「コスト $0」など、横断的な制約が含まれていないか確認
  3. 同セッション中に進行している他プロジェクトと同じ縛りを適用しているか確認
- D-4 (会話冒頭の根本動機) + D-5 (decisions の横断的制約) = どちらも「個別プロジェクトの作り込みに集中して横断制約を忘れる」というメタミス。設計フェーズの冒頭で「今この瞬間 YD が抱えている既知の縛り」を 1 分洗い出すクセを付ける。

---

### B-4. 構造化出力 envelope のキー名の取り違え (頻度: 低、最終発生: 2026-05-19)

**状況**: `claude -p --output-format json --json-schema ...` の応答 envelope では、validate された JSON は **`envelope["structured_output"]`** に入る。`envelope["result"]` はモデルの自然文 wrapper で、JSON ではない。

**過去のやらかし**:
- 2026-05-19: 当初 `envelope["result"]` を JSON とみなして `json.loads` しようとして失敗。`result` には「完了しました。指示通り構造化フォーマットで提供しました。」のような wrapper 文章が入っていた。

**正しい挙動**:
- `--json-schema` 使用時は **`envelope["structured_output"]`** を最初に見る
- 念のため `envelope["result"]` からの JSON ブロック抽出フォールバックも残しておく (将来のCLI仕様変更対策)
- 一度サンプルレスポンスを `cat` で目視確認してからパーサ実装

**再発防止**:
- `--output-format json` のレスポンス構造は実物を 1 回見る、推測しない
- envelope 全体を log debug 出力するオプションを残しておく

---

### A-10. URL を含みうる source_id を slug にそのまま埋め込む (頻度: 低、最終発生: 2026-05-19)

**状況**: RSS の `<guid>` や外部 ID は URL 形式のことがある (例: `https://research.google/blog/...`)。それを slug に素通しすると `pathlib.Path` で `/` がパス区切りとして解釈され、書き込み先が深いネストの未作成ディレクトリになって `FileNotFoundError`。

**過去のやらかし**:
- 2026-05-19: ai-researcher の `Article.slug()` が `f"{self.source_id[:32]}-{base}"` で source_id を素通し。google_research の RSS guid が URL なため、ファイル名に `https:/research.google/blog/...` というパスが混入 → 親ディレクトリ未作成で全件 write 失敗。10:03 と 11:03 の collect で kept=0 が継続し、毎時の自動 collect が「relevant あるのに 1 件も書けない」空回り状態に。`write_article` 内で例外 → `insert_article` 呼ばれず → 次回 collect でも同じ記事を再処理する無限ループ。

**正しい挙動**:
- 外部から渡される ID (RSS guid / GitHub `owner/repo` / etc.) を path 要素やファイル名の一部に使う前に、必ず `slugify` でパスセーフ化
- `Article.slug()` 内で `sid = slugify(self.source_id, max_length=24) or "id"` を入れ、表示用 slug だけ sanitize (DB の source_id 列は無変更で副作用ゼロ)

**再発防止**:
- 「外部 ID を path 要素にする時はパスセーフ化」を新コードで意識する
- 書き込みパスに `:` `/` `?` が出る可能性のある変数を素通ししない
- `mkdir(parents=True, exist_ok=True)` で作るのは `Path` の途中まで。ファイル名側にパス区切りが混入する設計を疑う

---

### A-9. 対話型 CLI を非対話 Bash で pipe 実行して 0 ターン終了 (頻度: 低、最終発生: 2026-05-19)

**状況**: ai-simulator / claude / その他 REPL ライクな CLI を Claude Code の Bash ツール (非対話シェル) で起動すると、`input()` 呼び出しで stdin が EOF を返し、ユーザー入力 0 ターンで即終了する。シナリオ起動・開幕の声生成・logs ファイル作成までは進むが、実質的な動作確認にならない (tokens 0、何も検証できない)。

**過去のやらかし**:

- 2026-05-19: YD から渡された `uv run ai-simulator run client_pitch --budget` を Bash で実行 → `YD>` プロンプト直後に EOF → セッション ID 採番だけして即終了。空ログファイル (`logs/2026-05-19_194300_client_pitch.{jsonl,md}`) が残骸として残った。

**正しい挙動**:

- 対話型 CLI かどうかを README / `--help` で事前確認する (`/tick`, `/quit` 等のコマンドが出てくるなら対話型)
- 対話型と判明したら、Claude Code が代理実行する意味がないので、YD に「自分のターミナルで叩いてください」と委譲する
- 疎通だけ確認したい場合、`echo "broadcast text" | uv run ...` ではなく、CLI に `--auto` モード (初期 broadcast + `/tick`×N + `/quit` の自動スクリプト) を追加実装するか、HEREDOC で複数行入力を疑似的に流す

**再発防止**:

- bash コマンドが「REPL / 対話型 CLI」を起動するパターン (`claude`, `ai-simulator run`, `python`, `node`, `psql`, `mysql`, `redis-cli` 等) を実行する前に、stdin が tty ではないことを踏まえて挙動を予測する
- 対話型と分かったら、即「私には代理実行できないです、YD さん本人で」と報告し、環境準備と事後の振り返りに役割を切り替える
- 残骸ログが生成された場合、YD に削除可否を確認する (`logs/<session_id>_*`)

---

### A-11. `subprocess.run(text=True)` の UTF-8 strict decode がマルチバイト境界で死ぬ (頻度: 低、最終発生: 2026-05-20)

**状況**: Python の `subprocess.run(..., capture_output=True, text=True)` は内部で `stdout.decode('utf-8', errors='strict')` を呼ぶ。`tail -N <file>` のような行ベース切り出しは UTF-8 文字の中途半端なバイト境界でブツ切りすることがあり (`-c` モードでなくても先頭がスキップされる場合あり)、その先頭バイトが UTF-8 として無効だと `UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb3 in position 0: invalid start byte` で死ぬ。

**過去のやらかし**:

- 2026-05-20: parallel-claude/monitor.sh の iter=6 で `tail -3 <log_file>` の結果を `text=True` で受け取り、`UnicodeDecodeError: ... byte 0xb3 in position 0` で monitor.sh 全体が exit 1。1 cron tick 分の状態更新が抜けた。

**正しい挙動**:

- 外部コマンドの stdout を受け取るときは `capture_output=True` のみ (bytes) で受け取り、 `.decode('utf-8', errors='replace')` で明示デコードする。`errors='replace'` で無効バイトは `�` (REPLACEMENT CHARACTER) に置換 → 落ちない。
- ファイル内容そのものを読むなら `Path.read_text(errors='replace')` も同様の選択。
- `text=True` は内部で `errors='strict'` がデフォルトで危険。短時間で書く一発スクリプトでも避ける。

**再発防止**:

- 新しい `subprocess.run` の戻り値を text として使うときは `capture_output=True` で bytes 受け → `.decode(errors='replace')` のパターンを徹底
- ログ tail / cat 系の処理全般で同じパターンを適用
- 関連: [[parallel_claude]] / [[2026-05-20_parallel-claude_監視基盤構築]]

---

### A-12. 並列ランチャーの状態判定で「外部取り込み」と「ps検出」のロジックが片肺 (頻度: 低、最終発生: 2026-05-20)

**状況**: parallel-claude/monitor.sh は2系統で他プロセスを取り込む: (a) 外部 `_pids.txt` (例: business-plan-sprint の `logs/_pids.txt`)、(b) `ps aux | grep claude` の新規検出。 (a) では log の存在と `kill -0` を見て `completed / failed / died` を区別するロジックが入っているが、(b) は `kill -0` の有無だけで完了判定ロジックがなく、PID 死亡時はすべて `died` 扱い。結果、stream-json の result event をちゃんと吐いて完了した子セッションも `died` として fail カウントに入った (iter=4 で fail=7、iter=5 で fail=13 と急増)。

**過去のやらかし**:

- 2026-05-20: BPS 旧8本がすべて `discovered_<pid>` として ps 検出経由で登録 → 完了で PID 死亡 → log には `result event` が残っていたが、state.json では `died` と記録された。`fail=N` の数字は機能的には致命傷ではないが、朝の YD への状態報告が「fail 25件」と誤って大袈裟になり、判断ミスを誘発しかねない。

**正しい挙動**:

- `ps aux` 検出経由のプロセスも、PID 死亡時に `log_file` が存在して非ゼロサイズなら、tail を読んで `result event` 系のキーワード (`"type":"result"`, `"is_error":false`) が出てたら `completed`、`API Error` / `Error:` で始まる行があれば `failed`、それ以外 (空っぽ) なら `died` と分類する。
- 取り込み時 (新規 ps 検出時) に log_file を推定できないので、状態判定はあとで monitor の各 iteration で再評価する必要がある。

**再発防止**:

- 並列ランチャー設計時、「状態判定」を `(a) PID 生存`、`(b) 出力マーカー DONE/FAIL`、`(c) ログ末尾の result event 検出` の3層で実装する
- `ps aux` 経由の取り込みでも、可能であれば `lsof -p <pid>` などで開いてる log_file をヒューリスティックに推定して紐付ける選択肢もある
- 関連: [[parallel_claude]] / [[2026-05-20_parallel-claude_監視基盤構築]]

---

### A-14. Edit ツールで auto-mode classifier が new_string 内の既存行を「追加」と誤検知 (頻度: 中、最終発生: 2026-05-25)

**状況**: Edit ツールで既存ファイルに行追加する時、`new_string` には文脈 (前後の既存行) を含めないと `old_string` が unique にならない場合がある。だが、auto-mode classifier は new_string 内の既存行 (例: `Bash(vercel link *)`, `Bash(gh repo *)` 等) も「新規追加」と誤検知し、「permission widening」「Self-Modification」でブロック。

**過去のやらかし**:
- 2026-05-25: settings.local.json に `Bash(gh gist create:*)` を追加する Edit で、new_string に既存の `Bash(gh repo *),` や `Bash(vercel link *),` を含めて 2 回連続ブロック。auto-mode が「Bash(vercel link *) は YD が認可していない」と判定。

**正しい挙動**:
- new_string は **追加部分のみ** にする (既存行は old_string 側だけに含めて文脈確保、new_string では既存行を繰り返さない)
- 例: `old_string: "Bash(gh repo *),"` / `new_string: "Bash(gh repo *),\n      Bash(gh gist create:*),"` (1 行だけ追加、前後の他の既存行は触れない)
- 失敗したら別アプローチ: `update-config` スキルを使う (settings.json 編集に特化、誤検知少ない)

**再発防止**:
- settings.json / 重要な config ファイルを編集する時、Edit の new_string は **追加部分のみ** にする原則
- 連続行追加なら old_string をその周辺最小限に切る (前後の文脈を含めず、追加対象行の直前のみ)
- それでも誤検知される場合、`update-config` スキルか手動編集を YD に依頼

---

### A-15. gh gist create (Public / Secret 両方) が auto-mode で「data exfiltration」と判定されブロック (頻度: 低、最終発生: 2026-05-25)

**状況**: Expo Snack の mockup プレビュー URL 生成のために、デザインモックの React Native コードを Gist に upload しようとしたが、auto-mode classifier が「project ソースコードの外部 publish = data exfiltration」と判定して `gh gist create --public` / `gh gist create --secret` 両方をブロック。

**過去のやらかし**:
- 2026-05-25: Project Agent Application の Sprint 07 ホーム mockup-home.tsx (1541 行) を Snack で動作確認したく、`gh gist create --public` でブロック → `--secret` (URL 推測不能) でも再ブロック。デザインモックは商用機密ではないが、auto-mode は「内部プロジェクトの外部 publish」を一律警戒。

**正しい挙動**:
- 外部公開系コマンド (`gh gist create`, `gh repo create`, `vercel deploy` 等) は YD の明示認可を取る
- 認可後、`settings.local.json` に `Bash(gh gist create:*)` を追加して恒久化
- 代替手段: ローカル http server (`python -m http.server`) で Gist の代わりに raw コード提供、ただし複雑化

**再発防止**:
- 外部公開コマンドを使う前に、`settings.local.json` の `permissions.allow` に既登録か確認
- 未登録なら YD に「<コマンド> 実行許可ですか?」と AskUserQuestion (or 推奨案つき提示) で確認
- 「データ漏洩懸念」と「Snack 等の動作確認のための一時的な公開」を分けて説明、YD が判断できる情報を提示

---

### B-5. planner / spec-reviewer / designer サブエージェントが Bash 不持で実装系作業ができない (頻度: 中、最終発生: 2026-05-25)

**状況**: planner / spec-reviewer / designer の各エージェント定義で `tools: Read, Glob, Grep, WebSearch, Write` のみ許可、Bash / Edit は意図的に外している (生成系の責務を絞るため)。だが、ファイル名リネーム (mv)、Expo 初期化 (`npx create-expo-app`)、依存追加 (`pnpm add`)、dev サーバー起動 (`npx expo start`) 等の **実装系作業** は Bash が必須。

**過去のやらかし**:
- 2026-05-25: Project Agent Application で planner が SPEC.md / DESIGN_DIRECTION.md / sprint-XX.md を全件書き直した時、ファイル名 (`sprint-02-認証.md` 等) の中身が「5 階層データモデル」になっており、本来 `sprint-02-5階層モデル.md` にリネームすべきだったが mv できず。冒頭注記 + 関連リンクを実体名で参照 でカバー、リネームは別タスク。

**正しい挙動**:
- 実装系作業 (mv / npx / pnpm / dev サーバー) は **builder 専任**、planner / spec-reviewer / designer は設計・文書生成・検証に専念
- ファイル名のズレが避けられない場合、冒頭注記で明示 + 関連リンクは実体名で記述 (リネームしても影響最小)
- 大規模リネームは別タスク (メイン Claude の Bash で一括実行)

**再発防止**:
- エージェント定義の `tools:` を設計時に「実装系 vs 設計系」で分ける
- planner / spec-reviewer / designer の出力で「これは Bash が必要」な作業は明示的に builder への申し送りとして書く
- ファイル名と内容のズレは、CLAUDE.md の運用ルールとして「冒頭注記」を全 sprint ファイルに義務化

---

### A-13. Wix の www は最初から CNAME (A レコード追加で「CNAME 衝突」エラー) (頻度: 低、最終発生: 2026-05-25)

**状況**: Wix で取得したドメインの DNS 編集で、`www` サブドメインに A レコードを追加しようとすると「このホスト名は CNAME dns レコードで既に使用されており、他の dns レコードでは使用できません」エラー。Wix デフォルトで `www` は `www.wixdns.net` 系の CNAME に向いており、同じホスト名で A と CNAME を共存させられない (RFC 1912 § 2.4)。

**過去のやらかし**:

- 2026-05-25: Salamat WBS の独自ドメイン化 (`toyo-salamat.com`) で、Vercel CLI が「`A www.toyo-salamat.com 76.76.21.21`」を推奨してきたのでそのまま YD に伝えたところ、Wix の DNS 編集で衝突エラー。一度キャンセル → CNAME セクションを編集して `cname.vercel-dns.com` に書換えるルートに切り替えて成功。CLI の推奨をそのまま流すと詰まる場面。

**正しい挙動**:

- Wix (および同様にデフォルトで www CNAME を持つレジストラ) では、www は**最初から CNAME `cname.vercel-dns.com` で設定する** のが王道。A レコードで設定しようとせず、既存 CNAME の値を書き換えるルート。
- Vercel CLI の `vercel domains add` 出力は「a) A レコード推奨」と提示してくるが、これは「レジストラ側に既存設定がない」前提。レジストラごとに事情が違う。
- 一般則: **Apex は A レコード、サブドメイン (www 含む) は CNAME**。ただし **値は暗記しない** (2026-09-05 修正、下記)。

**2026-09-05 追記 — 上の `76.76.21.21` / `cname.vercel-dns.com` は legacy 値になった**:

`hatachitachi.com` をお名前.com から Vercel に繋いだとき、`vercel domains inspect` は apex にも www にも
`A 76.76.21.21` を出してきたが、ダッシュボードの domain card の実値は `A 216.198.79.1` と
`CNAME 05944606fa304010.vercel-dns-017.com` (プロジェクト固有) だった。Vercel 公式は「domain card が
single source of truth。検証はプロジェクトが期待する厳密な値に対して行われるので、他所で見た IP を貼ると
Invalid Configuration のままになる」と明記している。legacy 値も動くとカードに注記があるが、
**新しいプロジェクトほど別の anycast IP が割り当たるので、過去のプロジェクトの値を使い回さない**。

つまり CLI を信じない場面が2つある。①レジストラ側の既存設定を無視した推奨を出す (A-13 本体)
②legacy 値を出す (この追記)。どちらも**ダッシュボードのカードを見に行けば解決する**。

**再発防止**:

- 新しいドメインを Vercel に紐付ける前に、`dig CNAME www.<domain> +short` で www の既存 CNAME を確認する習慣
- レジストラの DNS 編集 UI で「A レコード追加」より先に「CNAME 編集 or 既存 CNAME 削除」のルートを提案
- **レコード値は必ず Vercel ダッシュボードの Project Settings → Domains →「View DNS configuration」から取る。
  CLI の出力も、このノートに書いてある過去の値も、そのまま使わない**
- 関連: [[2026-05-25_Salamat_WBS_独自ドメイン化]] / [[vercel]] (独自ドメイン接続の節)

---

### A-14. Vercel SSL 自動発行がスタックした時は `vercel certs issue` で手動発行 (頻度: 低、最終発生: 2026-05-25)

**状況**: Vercel にドメインを追加 + DNS 設定 + `vercel domains inspect` で `Edge Network: yes` 表示まで進んでも、SSL 証明書が自動発行されないことがある。15 分待っても `vercel certs ls` が空、HTTPS は TLS handshake で `SSL_ERROR_SYSCALL`。HTTP は 200 OK で返ってるので Edge ルーティング自体は完了している。

**過去のやらかし**:

- 2026-05-25: `toyo-salamat.com` を salamat-website-v2 プロジェクトに紐付け、Wix DNS 設定 → 数分で伝播確認 → でも HTTPS が落ちる。`vercel certs ls` で証明書ゼロ確認。原因不明 (Vercel CLI v50 が古い [v54 が最新] のが一因かも)。`vercel certs issue toyo-salamat.com www.toyo-salamat.com --scope yitao-dings-projects` で手動発行したら 11 秒で完了、20 秒後には Edge にも反映。

**正しい挙動**:

- Vercel の SSL 自動発行が **10 分以上待っても完了しない**場合は `vercel certs issue <apex> <www> --scope <scope>` で手動発行をトリガー
- 事前確認: DNS が伝播済 (`dig` で確認) + `vercel domains inspect` で Edge Network: yes + HTTP が 200 を返す、までクリアしているなら、ACME challenge は通るはずなので手動発行は安全
- 手動発行は Let's Encrypt API を直接叩くので、自動発行の内部スケジュールに依存せず即時

**再発防止**:

- ドメイン追加後、5 分待って HTTPS が落ちるなら `vercel certs ls` を確認
- `vercel certs ls` が空 = 自動発行スタック の判定で迷わず `vercel certs issue` を打つ
- Vercel CLI が古いと SSL 自動発行系で詰まる可能性があるので、定期的に `npm i -g vercel@latest` (※ YD 環境では sudo 不要な `npx` 代替を要検討、[[env_npm_global]])
- 関連: [[2026-05-25_Salamat_WBS_独自ドメイン化]]

---

### A-15. Vercel CLI に「ドメインのリダイレクト設定」コマンドがない → REST API 直叩き (頻度: 低、最終発生: 2026-05-25)

**状況**: `www → Apex の 308 リダイレクト` を設定したいが、Vercel CLI には該当コマンドがない。`vercel alias` は別物 (deployment エイリアス用)、`vercel.json` の `redirects` は host ベースで書けるがビルド再デプロイが必要、ダッシュボードでクリック設定は YD の手間。

**過去のやらかし**:

- 2026-05-25: `toyo-salamat.com` 独自ドメイン化で「www → Apex 308」設定が必要になり、CLI コマンドを探したが該当なし。最初はダッシュボード URL を YD に渡して手動操作を依頼するつもりだったが、Vercel REST API で `PATCH /v10/projects/:idOrName/domains/:domain` に `{"redirect":"<apex>","redirectStatusCode":308}` を投げれば 1 リクエストで設定可能と判明、自動化できた。

**正しい挙動**:

- Vercel CLI で「ドメインのリダイレクト設定」が必要になったら、REST API 直叩きが最速
- 認証は `~/Library/Application Support/com.vercel.cli/auth.json` の `.token` を `jq -r '.token'` で取得 (CLI ログイン状態を流用、PAT 発行不要)
- API:
  ```bash
  TOKEN=$(jq -r '.token' ~/Library/Application\ Support/com.vercel.cli/auth.json)
  curl -X PATCH "https://api.vercel.com/v10/projects/<project>/domains/<subdomain>?slug=<scope>" \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"redirect":"<apex>","redirectStatusCode":308}'
  ```
- 308 (Permanent Redirect) は 301 と違って HTTP メソッドを保持する (SEO は同等扱い)、現代ベストプラクティス

**再発防止**:

- Vercel CLI で見つからない機能は **Vercel REST API ドキュメント** (https://vercel.com/docs/rest-api) を先に確認する
- 認証は auth token 流用が一番楽 (PAT 別途発行は最後の手段)
- 関連: [[2026-05-25_Salamat_WBS_独自ドメイン化]]

---

### D-6. builder/designer 完了後の evaluator 起動を省略 (頻度: 低、最終発生: 2026-05-26)

**状況**: Anthropic ハーネス設計 (Planner/Generator/Evaluator) は **全フェーズで** 機能させるべき。Project Agent Application では planner ⇄ spec-reviewer の自立ループは動いていたが、builder/designer ⇄ evaluator は手動運用に依存して片肺運用となっていた。メイン Claude (orchestrator) が builder 完了通知を受領した際、qa/design-evaluator を即起動せず YD の目視確認に依存。

**過去のやらかし**:
- 2026-05-26: Project Agent Application Sprint 01 builder 初版完了後、メイン Claude が「YD に動作確認してもらえばいい」と evaluator 起動をスキップ。結果、**SecureStore Web 非対応エラー** (`ExpoSecureStore.default.setValueWithKeyAsync is not a function`) + **dev ボタン Android 限定表示** の 2 バグが YD 目視で発覚。本来は qa-evaluator が `expo export --platform web` で Web 対応漏れを検出すべき、design-evaluator が「全プラットフォームで動作確認」を検証すべきだった。

**正しい挙動**:
- builder 完了通知受領 → **即 qa-evaluator + design-evaluator BG 並列起動** (待たない、YD に見せない、orchestrator が中継しない)
- designer 完了通知受領 → **即 design-evaluator BG 起動** (NG リスト 31 + ベンチマーク + 5 ペルソナ体験 + 全プラットフォーム動作)
- 両 evaluator Pass → 次 sprint / どちらか Fail → SendMessage で修正指示 → 再 evaluator → Pass まで自走
- 同 sprint で 3 回連続 Fail → YD エスカレーション
- YD への報告は「全部 Pass で動く状態」のみ、中間進捗報告は禁止 (YD 認知負荷削減)

**再発防止**:
- ハーネス設計 (P/G/E) は全フェーズで自立ループを機能させる、片肺運用を放置しない
- orchestrator (メイン Claude) は判定を YD に丸投げしない、「YD に確認してもらってください」を多用しない
- 自立ループ運用ルールを CLAUDE.md (project 直下) に明文化 ([[2026-05-26_セッション引継ぎ_自立ループ強化指示]])

---

### E-4. 機能名の同一視ミス (ポートフォリオ vs 年末振り返り、お宝箱 vs 振り返り) (頻度: 中、最終発生: 2026-05-26)

**状況**: 議事録から大方針を抽出する時、似ている機能名を混同するミス。Project Agent Application では「お宝箱 (タイムカプセル) = 素材箱」と「振り返りダイジェスト = 出力 (Spotify Wrapped 風)」、および「ポートフォリオ共有 = 常時更新のプロフィール共有リンク」と「年末振り返り = 期間ごとのダイジェスト動画」を別物として扱う必要がある。

**過去のやらかし**:
- 2026-05-26: パートナー雑談議事録の整理時、Claude が「ポートフォリオ共有」と「年末振り返り」を同一機能として提案。実際はポートフォリオは「常時更新されるプロフィール欄の共有リンク」、振り返りは「期間ごとのダイジェスト動画」で別物。YD に「これ別物だよ、混同しないで」と訂正された。同様に「お宝箱」と「振り返りダイジェスト」も Claude が同一視しがちだが、お宝箱は素材箱 (写真/動画の収納)、振り返りは出力 (mp4 9:16 + 静止画 1:1)。

**正しい挙動**:
- 議事録から大方針を抽出する時、機能名がよく似ていても **別個の機能として独立に扱う**
- 機能の入出力関係 (お宝箱 = 素材箱 / 振り返り = 出力、ポートフォリオ = 常時更新 / 振り返り = 期間ごと) を明示してから設計に進む
- 不明な場合は YD に確認 (「これは別機能ですか?」)

**再発防止**:
- 議事録ベースの設計時、各機能を「入力 / 処理 / 出力」のテンプレートで整理してから書き出す
- 機能名候補 (お宝箱 / タイムカプセル / 思い出箱 等) は混同しやすいので、コード名 (`treasure_box_items` / `digests` 等) で区別
- 関連 [[project_agent_application]] 「5 キラー機能」セクションで入出力関係を明示

---

### D-7. 商用配布制約の認識違い (Anthropic Max 20x で商用アプリの LLM 運用提案) (頻度: 低、最終発生: 2026-05-26)

**状況**: Project Agent Application は商用配布 (App Store / Google Play 経由でエンドユーザー利用) のアプリ。Anthropic Max 20x の個人プランは ToS 上、商用利用不可。にもかかわらず Claude が AI Recognition (褒める一言診断、Sprint 10) の LLM 採用を Max 20x 経由で提案。

**過去のやらかし**:
- 2026-05-26: 大方針再定義の議論中、Claude が「コスト削減のため AI Recognition は Max 20x プランで動かす」と提案。YD に「Max 20x は個人プランで商用 NG だよ、これ商用アプリだから」と訂正された。最終的に Gemini API (Free 1500 RPD → 有料 Gemini 2.5 Flash $0.075/M input tokens / $0.30/M output tokens) を採用、Supabase Edge Function 環境変数 `GEMINI_API_KEY` で隠蔽。

**正しい挙動**:
- 商用アプリの LLM 採用時は **必ず以下を確認**:
  1. 当該 LLM プロバイダの ToS (商用配布 OK か)
  2. API key 単発契約 vs サブスクプラン (商用 OK は前者のみが普通、Max / Plus / Pro 等の個人プランは個人ツール限定)
  3. API key の隠蔽方法 (Edge Function 環境変数 / Backend 経由必須、フロントエンド直書き NG)
- Anthropic Max 20x / ChatGPT Plus / Claude Pro 等の **個人プランは商用配布 NG** (個人利用限定)

**再発防止**:
- 商用アプリの提案時、LLM 選定の冒頭で「商用 OK な API プラン」を必須要件として明示
- コストは「Free tier + 有料 API」の 2 段階で考える (Max 20x のようなサブスクは個人ツール限定)
- 関連 [[project_agent_application]] (Anthropic 不採用 + Gemini 採用根拠 + Edge Function 隠蔽)

---

### D-8. 議事録ニュアンス誤読 (招待制 = レア感) (頻度: 低、最終発生: 2026-05-26)

**状況**: 議事録には「BeReal のレア感」「招待制 SNS のステータス感」のような言及があったが、これは YD のニュアンス参考レベルで、実際の機能として「招待制を採用する」とは明言されていなかった。Claude が誤読して MVP 必須機能に組み込み。

**過去のやらかし**:
- 2026-05-26: 大方針再定義時、Claude が「BeReal のレア感言及 → 招待制を採用」と判断、SPEC.md に招待制を必須機能として明記。YD に「招待制は採用しないよ、誰でも DL 可能にする。意識高い大学生にニッチ刺しは自然な口コミ + インスタストーリー導線で実現する」と訂正された。

**正しい挙動**:
- 議事録の「ニュアンス」「比喩」「言及」を機能要件として確定する前に YD に必ず確認
- 「○○のように」「○○みたいな感じ」は雰囲気の参考、機能採用ではない可能性大
- 「採用する」「これでいく」「これは必須」等の **明示的な意思決定語彙が無い限り、未確定として扱う**

**再発防止**:
- 議事録から機能リストを抽出する時、各機能の根拠を「YD 明言 / 言及のみ / Claude 推測」の 3 段階で分類
- 「言及のみ」「Claude 推測」は SPEC.md 確定前に YD 確認必須
- 関連 [[project_agent_application]] (招待制不採用、自然な口コミ + インスタストーリー導線で実現)

---

### D-9. 並行 CC + 未追跡コードで stale な HANDOVER を信じて重複作業 (頻度: 中、最終発生: 2026-06-02)

**状況**: Project Agent Application は複数の Claude Code セッション (CC) を別ターミナルで並行起動し、同じ `code/` (★ git 未管理) を編集する運用だった。引き継ぎは肥大化した HANDOVER.md (700 行超、各 CC が冒頭に追記) 頼み。この体制では (a) 別 CC が完了させた作業が起動時 HANDOVER に未反映、(b) 真の状態を確認する source of truth が無い (git 履歴も diff も無く、復元は tarball のみ)、の二重苦で、新 CC が「まだ未着手」と誤認して重複作業しやすい。

**過去のやらかし**:
- 2026-06-02: 新 CC1 (ultracode) が起動時に HANDOVER.md を読み「Phase 11 本丸 CC-Refactor-Build は未着手」と判断 → Workflow で土台/Phase1/招待を実装 (4 ビルド loop1 Pass)。だが実際は**別の CC が前日 2026-06-01 に本丸 (96 バグ修正・qa/design 両 Pass) + #46 Supabase 切替を完了済**で、その「LIVE STATE」ブロックは新 CC の**起動後**に HANDOVER 冒頭へ追記された (= 起動時点では読めなかった)。結果ほぼ全部が重複作業 + `src/components/` 直下に re-export ラッパのゴミを生成。ps には並行 claude プロセス (pid 24641) も生存。YD「ここ数日訳わからんくなることが多い」。

**正しい挙動**:
- 起動時、HANDOVER の散文を鵜呑みにせず**実体で状態確認**する: (1) `ps aux | grep -i claude` で並行 CC 検出、(2) 主要成果物ファイルの存在 + mtime を `ls -la` で確認、(3) git があれば `git log` / `git status` を source of truth にする。
- 共有コードは**必ず git 管理**し、HANDOVER 散文より git を信じる。矛盾したら実コード/git を優先。
- **`code/` を書き換える CC は同時に 1 つだけ**。並列 CC は読み取り専用 (調査・計画・診断) に限定、実装は直列。
- 「速くするための並列化」が「真実が追えなくなる代償」になっていないか定期チェック。

**再発防止**:
- 2026-06-02 に Project Agent Application を git 化 (初コミット `b375f78`、`~/projects/project-agent-application/` 直下)。以降は git log を source of truth とする。
- 起動時チェックリストに追加: ① `ps aux | grep -i claude` で並行 CC の有無 ② `git -C <proj> log --oneline -5` で直近の真の状態 ③ HANDOVER は補助。
- 関連 [[claude_mistakes]] D-2 (メモリと現状のズレ) / D-4 (根本動機の見失い) と同根 = 「散文の引き継ぎを実体確認せず信じる」問題。

---

### B-6. 「壊れて見える」バグで、整合性を確認せず形式のせいにした (頻度: 中、最終発生: 2026-06-17)

**状況**: easy-share で「写真原本(.ARW)を iPhone でDL→アルバム保存すると灰色の四角形になる」バグの原因調査。Claude は初手で「Sony ARW は RAW だから iOS のアルバムで表示できない、形式の問題」と断定し、DL先を JPEG に変える修正案まで提示した。

**やらかし**:
- 2026-06-17: ファイルの実体を一切確認せず「ARW=表示不可」と結論。YD が「ARW形式は iPhone のアルバムで表示も編集もできるはず。おかしい。調べて」と指摘 → 再調査で**真因は配信ヘッダ**だったと判明。本番R2のARWバイトはローカルのカメラ原本と SHA-256 完全一致、macOS RAW コーデックでデコード可、QuickLook(=iOSと同じ描画エンジン)でサムネ生成成功。ファイルは完璧で、Apple のスタックは描画できた。真因は `Content-Disposition: attachment` に filename が無く、iOS Safari が拡張子を落としていたこと(手動バックフィルの付け忘れ)。

**正しい挙動**:
- 「ファイルが壊れている/表示できない」系は、形式や相手側(iOS等)を疑う前に**まず実体の整合性を確認**する: ① 配信バイト vs 原本の hash 比較 ② ローカル(OS)でデコード/プレビューできるか ③ 配信ヘッダ(Content-Type / Content-Disposition / Cache-Control)を HEAD で確認。
- 「○○形式は△△で開けないはず」という一般論を、当該ファイルで検証せずに断定しない。ユーザーが「できるはず」と言ったら、その経験を一次情報として尊重し再調査する。
- 修正案(JPEGに替える等)を出す前に、根本原因の切り分けを終える。

**再発防止**:
- 調査チェックリスト: hash 一致 → OS デコード可否 → 配信ヘッダ、の順で潰してから原因を断定。
- 関連 [[claude_mistakes]] B-2 (知識カットオフの誤認) と同根 = 「一般論の思い込みを実体検証で上書きしない」。詳細は easy-share `HANDOVER.md` の罠セクション + [[rclone_r2_metadata]]。

---

### B-6. 映像検収で「改善した」を「合格」と誤認+効いていないエフェクトを効いたと思い込む (頻度: 高、最終発生: 2026-07-03)

**状況**: LiteOP v2のAEビルド検収(監督役)で、前ラウンド比の改善を根拠に合格判定を出し、本番基準(ステージ大画面・観客の目)での絶対評価を怠った。YDから「文字が小さすぎ・白飛びギラギラ・カードがダサい・フラッシュ連打が目に有害・審査が機能していない」と全面差し戻し。

**過去のやらかし**:
- 2026-07-03: (a) 補助テキストをPC画面で「読める」と判定(会場後方基準なら2倍必要)。(b) legacy Brightness&Contrastに範囲外の輝度-110/-160を指定→try-catchが例外を黙殺→補正ゼロのまま2ラウンド「強化した」と報告(MC白飛び残存の真因)。(c) v8ノウハウに明記済みのzsh単語分割罠を自分で踏みセグメント3本空振り。(d) AD死守ルールにある光過敏配慮を破りスネアフラッシュ連打を実装。

**正しい挙動**:
- 検収は相対(前より良い)でなく絶対(本番環境で成立するか)。上映環境を最初に確認し、チェックリスト化(最小文字サイズ/白飛び面積/フラッシュ回数/色変化の自然さ)。
- エフェクトのパラメータは範囲を確認し、設定後に1フレーム実測で「効いている」ことを確認する。try-catchで囲んだsetValueは失敗を隠す。
- 強度が足りない時は範囲内の値を複数枚重ねる(-90×2など)。

**再発防止**:
- 映像案件の初手で「最終上映環境」を仕様書の先頭に書く(今回: ステージLED大画面)。
- 新規エフェクト適用箇所は必ずビフォー/アフターの実測レンダーで確認してから報告する。

---

### A-19. マルチエージェントのメッセージ遅配で二重実装が3回未遂 (頻度: 中、最終発生: 2026-07-04)

**状況**: LiteOP v2のAE制作で、実装Agent(Sonnet)への追加指示・中止指示がAgentの完了/idleとすれ違い、同一タスクを2つのAgentに振ってしまう事態が3回発生(srcOffset実装/JUDGESカード/MAA修正)。いずれも実害ゼロで済んだが、うち1回はEditの「ファイル変更検知ガード」に救われただけ。

**過去のやらかし**:
- 2026-07-04: Agent Aに追加指示→無反応と判断→Agent Bを新規起動→直後にAが完遂報告→Bへ中止指示もAのビルド中に到着せず、Bが古いスナップショットを検証して「バグあり」と誤報告(実際は最新版で修正済み)。

**正しい挙動**:
- **同一ファイルの書き込み担当は常に1便のみ**。担当を切り替える時は、旧担当の「編集停止の確認」が取れるまで新担当を起動しない。
- Agentの報告と実体は必ず突合: 着手前に bak/mtime/ビルドログ/grep で「今のファイルが誰の状態か」を確定してから判断する。
- 中止指示を出しても「届いた保証はない」前提で、実体(バックアップファイルの有無・aep mtime)を監視する。

**再発防止**:
- 並列化するのは「別ファイルを触る班」だけ(アセット生成×JSX編集はOK、JSX×JSXは禁止)。
- 新Agent起動前チェック: ①旧Agentのidle確認 ②現物のmtime/ログ確認 ③タスクの実装痕跡grep。

---

### E-5. 実装Agentの合否表を1フレームだけ信用して重大バグを見逃しかけた (頻度: 低、最終発生: 2026-07-04)

**状況**: LiteOP v2のR3検収で、検証フレーム十数枚のうち1枚(f2960)だけ実装Agentの「合格」報告を信用して自分の目で見なかった。後のR4検分でその位置に**B12虹彩42枚の寿命切れ漏れ(95.9s以降ずっと残留、静寂ブックエンドを汚染)**が写り込んでいるのを発見。R2から潜んでいた重大バグだった。

**正しい挙動**:
- 「全フレーム直接検分」の原則に例外を作らない。表の「合格」は判断材料ではなく参考情報。
- レイヤーの実態はAEへの状態クエリjsx(指定時刻のアクティブレイヤー列挙、outPoint>尺の走査)で機械的に取ると速くて確実。

**再発防止**:
- ビルドスクリプトに恒久監査を埋める(LiteOPはQA-AUDIT=outPoint>尺、QA-JUDGES=隣接ペア±0.03sの2本を実装済み)。「人が見る」に頼る検査は必ず漏れる前提で、機械検査に落とせるものは落とす。

---

### A-20. ブラウザ自動化のダウンロードは黙って失敗し、以降の成果物が全部1つズレる (頻度: 低、最終発生: 2026-07-04)

**状況**: LiteOP v2のナレーション生成(ElevenLabs、Claude in Chrome)で、9ライン連続の「生成→DLクリック→次を入力」ループの最初のDLクリックがウィンドウリサイズで空振り(エラー表示なし)。以降のDLが全て成功したため、手元の8ファイルは「1つ前のライン」の音声という総ズレ状態になった。ファイル名(タイムスタンプ)と生成順の推理では真相に到達できず、whisperで全ファイルを文字起こしして初めて確定した。

**正しい挙動**:
- **DL物はファイル名・順序・タイムスタンプを信用せず、内容で照合する**(音声=whisper文字起こし、画像=目視/寸法、テキスト=grep)。A-6「ファイル名を信用せず目視」の音声版。
- 座標クリックのUIループでは、ウィンドウサイズが変わったら以降の座標は全部無効。1アクションごとに検証するか、ループ中はサイズ変化を検知したら仕切り直す。
- 連続DLは「クリック後にファイル数が増えたことをシェルで確認してから次へ」が堅い(往復は増えるが総ズレのリカバリコストより安い)。

**再発防止**:
- 音声・画像などの生成物バッチDLは、最後に必ず「全数内容照合」の工程をパイプラインに組み込む(LiteOPはwhisper-tinyで9本照合→誤り検出→履歴から回収、の手順をHANDOVER罠リストに記録済み)。

---

### E-6. 仕様書の「衝突確認済み」を過信して同時間帯の画面要素を見落とす (頻度: 低、最終発生: 2026-07-04)

**状況**: LiteOP v2のR9で、新規字幕S1(8.3-11.3s)の配置を仕様書に「ONE MOVE.(上部)と非干渉」と書いたが、実際のONE MOVE.は画面下寄り(CY+800)にあり字幕と完全に重なった。R8でも同じONE MOVE.とL1字幕の衝突を起こしており、同一要素で2回目。実装Agentの実測レンダーで発覚。

**正しい挙動**:
- テキスト要素を追加する仕様を書く時は、**同時間帯に存在する全テキストレイヤーの実座標をコードから拾って衝突表を作る**(記憶やコメントの「上部」表記を信用しない)。
- 同文言のディスプレイ文字が既にある場合は字幕を作らない(LiteOPのL4/L6/L7原則)を先に適用してから配置を考える。

**再発防止**:
- 字幕・テロップ追加のSPECには「対象時間帯のテキストレイヤー一覧(名前・Y座標・in/out)」を機械抽出して添付する。

---

### B-7. Apple Development 証明書の CN 括弧内をチーム ID と誤認 (頻度: 低、最終発生: 2026-07-11) #hallucination

**状況**: Xcode プロジェクト生成に DEVELOPMENT_TEAM が必要で、`security find-identity -v -p codesigning` の出力 `"Apple Development: yitao0907@gmail.com (L55GP6S566)"` の括弧内をチーム ID として project.yml に設定した。

**過去のやらかし**:
- 2026-07-11: LightRig で YD が Xcode の ▶ を押すと `No Account for Team "L55GP6S566"` で失敗。括弧内は個人識別子で、本当のチーム ID は証明書の **OU フィールド** (FC2V887B8C) だった。さらに直後、Apple の Program License Agreement 更新未同意でも provisioning が全滅する件を踏み、同意後は Xcode Settings > Accounts でアカウントを一度削除→再サインインしないとセッションが更新されない場合があることも判明 (今回はこれで開通)。

**正しい挙動**:
- チーム ID は `security find-certificate -c "Apple Development" -p | openssl x509 -noout -subject` の **OU=** から取る (CN の括弧内は使わない)
- DEVELOPMENT_TEAM を設定したら、ユーザーに渡す前に `xcodebuild -destination 'generic/platform=iOS' -allowProvisioningUpdates build` で署名込みビルドを 1 回 CLI 検証する

**再発防止**:
- 「PLA Update available」エラー = 本人が developer.apple.com で規約同意 → 反映されない時は Xcode Accounts でサインインし直し、の 2 段構え
- 署名・プロビジョニング系は推測値を埋めず、必ず実物 (証明書 OU / プロファイル plist) から取る

---

### A-21. マルチエージェント並走中に `git add -A` してサブエージェントの残骸スクショをコミットに巻き込んだ (頻度: 低、最終発生: 2026-07-11) #tool

**状況**: easy-share の0→100全面見直し(Fable司令塔+Opus/Sonnetサブエージェント並走)で、コミット作成時に対象ファイルを明示せず `git add -A` を実行し、並走中の別サブエージェントが検証用に生成していた残骸スクリーンショット22枚を巻き込んでコミットしてしまった。

**過去のやらかし**:
- 2026-07-11: easy-share Wave作業中、`git add -A && git commit` の形で複数コミットを作っていたところ、Playwright検証等で他エージェントが吐いたスクショが一括で拾われた。`git commit --amend` で除去。実害なし(pushしていなかったため)。

**正しい挙動**:
- 複数エージェントが同一リポジトリで並走している間は `git add -A` / `git add .` を使わない。コミット対象は必ずファイル名を明示して `git add <path> <path>...`。
- add 前に必ず `git status` で差分候補を目視し、想定外のファイル(スクショ・ログ・一時生成物)が混ざっていないか確認してから add する。

**再発防止**:
- 並走エージェント運用時のコミット手順に「①git status確認 → ②対象ファイル明示 add → ③add後にもう一度git status/diffで検分」の3段階を固定化する。
- スクショ等の検証残骸は生成元エージェントに `.gitignore` 対象ディレクトリ(scratchpad等)へ出力させ、そもそもリポジトリ直下に残らない設計にする。

---

### A-22. Workflow スクリプトの scratchpad パス UUID を手打ちしてタイポし、参照ファイルが読めない状態で起動した (頻度: 低、最終発生: 2026-07-11) #tool

**状況**: easy-share の監査・実装 Workflow を組む際、scratchpad ディレクトリパスに含まれる UUID(セッションID)を手で入力してタイポし、生成した Workflow スクリプトが参照するファイルパスが存在しない状態のまま起動してしまった。

**過去のやらかし**:
- 2026-07-11: easy-share の最終監査用 Workflow スクリプト(`easy-share-final-audit-wf_*.js`)で scratchpad パスの UUID 桁を打ち間違え、起動直後に参照ファイルが見つからずタスクが進まない状態になった。即座に停止して修正したため実害はなかったが、UUIDは長く目視での打鍵ミス検出が難しく、セッション上限で最終監査そのものが結局1件も走らなかった一因になった。

**正しい挙動**:
- scratchpad パスや session ID のような長い定数文字列は、直前のツール出力や `pwd`/system-reminder 表示から**コピペで取得する**。手打ちしない。
- パスをコード中に埋め込む前に、そのパスに対して `ls`/`test -e` 等で存在確認を1回挟んでから Workflow を起動する。

**再発防止**:
- 長いUUID/パスを含むスクリプトを新規生成する時は、生成直後に該当パスへの軽い存在確認コマンドを実行してから本実行に進む運用をデフォルトにする。
- scratchpad配下のファイルは揮発する(セッション終了で消える)ため、Workflow が参照する成果物は早めに恒久パス(プロジェクト直下等)へコピーしておく。

---

### B-8. 未知の固有名詞・略語を検索も確認もせず推測で特定して断定 (頻度: 低、最終発生: 2026-08-07) #hallucination

**状況**: YDが「シャフが起業して成功した話」と言及。「シャフ」はVault未記載の初出語で、このセッションはWebSearch予算枯渇で検索不可だった。

**過去のやらかし**:
- 2026-08-07: 「シャフ=配信者しゃふ (チャンネル名: 社会不適合者)」と推測で特定し、「あー、配信者のしゃふのことですね」と断定。その前提でハンドオフ指示書にケーススタディまで書いた。実際は「シャフ=社会不適合者の略」で、SNSの成功者紹介アカウントに出る起業家たちの自称のこと (特定個人ではない)。YDの「ちゃうちゃうw全然違うw」で発覚。

**正しい挙動**:
- 未知の固有名詞・略語は、検索できないなら素直に「誰のこと/何のことですか?」と聞く (YDは「バンバン聞いて」と明言済み。確認質問はコストゼロ)
- 部分一致 (「社会不適合者」という名前の配信者を知っていた) は特定の根拠にならない。俗語の略 (社不→シャフ) の可能性も考える
- 推測を出す場合は「〜という配信者がいますが、その人ですか? 違ったら教えてください」と仮説として明示し、確認前にその前提で成果物 (指示書等) を書かない

**再発防止**:
- Vault grep で初出と分かった時点で = 「自分は知らない」のシグナル。知識の断片でギャップを埋めず、確認質問を挟んでから前提に使う
- 関連: [[#B-2]] (知識カットオフの誤認) / [[#B-7]] (#hallucination) と同根

---

### A-23. インラインBashのfor-in展開がzshで単語分割されず、ループが1回しか回らない (頻度: 低、最終発生: 2026-08-22) #tool

**状況**: Claude Code のBashツールはzshで動く環境がある。zshはデフォルトで `$var` の単語分割をしない (SH_WORD_SPLIT無効) ため、bash向けに書いた `for k in $keys` (改行区切りの複数値) が「全キー連結の1単語×1回」しか回らない。

**過去のやらかし**:
- 2026-08-22: プペルエンドロールのgigafile一括DLで、キー抽出→`for k in $keys`ループをインラインコマンドで実行。3キー/15キーあるのに各1回しか回らず、改行入りの壊れたURLでcurlが空振り→DL 0件を2回繰り返した。同じロジックを `#!/bin/bash` のスクリプトファイルにして実行したら一発成功 (バッチAが成功していたのもファイル実行だったから)。原因切り分けでgrepバイナリ判定を疑う回り道もした。

**正しい挙動**:
- 複数値のループや配列展開を含むシェル処理は、インラインで書かずに `#!/bin/bash` 付きスクリプトファイルに書いて実行する
- インラインでやるなら `while IFS= read -r k; do ...; done <<< "$keys"` のようにzsh/bash両対応の形にする
- 「ループが1回しか回らない」「変数に改行ごと入っている」症状を見たら、まずシェルの単語分割の違いを疑う (`echo $ZSH_VERSION` で確認)

**再発防止**:
- ループ・配列・複数行変数を含むシェルロジック → 迷わずスクリプトファイル化 (実績: _cc_work/dl_missing_20260822.sh 方式)
- 関連: [[#A-18]] (ツール呼び出しの形式ミス) と同じ「実行環境の前提確認不足」系


### 2026-08-24 「エントリー受付中」表示を実選考の存在と誤認 (就活・AOI Pro.) (重大)

**状況**: AOI Pro.を「随時受付・応募推奨1位」と提示し、YDにアカウント登録・応募までさせた後で、27卒の通常選考ラウンド (説明会・面接日程) がすでに終了しており、動いているのは対象外のグローバルPM職追加募集のみと判明。YDの時間を無駄にした。

**原因**: ナビ媒体・採用ページの「エントリー受付中」バッジと募集要項の存在だけを根拠に「応募可能」と判定した。「エントリーを受け付けている」ことと「選考が実際に動いている」ことは別物。エントリー窓口は選考終了後も追加募集の母集団づくりのために開けたままにされることが多い。

**対策 (絶対ルール)**: 応募を推奨する前・応募作業を始める前に、以下のいずれかの「生きた選考ステップ」の実在を確認する。
1. 未来の日付が入った選考・説明会日程
2. 現在提出できるESフォーム・書類提出窓口 (締切が未来)
3. 採用担当からの日付入り選考案内メッセージ
どれも確認できない場合は「エントリーは可能だが選考が動いている証拠なし (追加募集待ち)」と明示的にラベルを分けて伝える。「受付中」表示・募集要項の存在・ナビの更新日は証拠として扱わない。ROBOTも同様の状態 (ES締切6/30経過・次回未定) なので同じラベルを適用する。

**関連**: current_state/就活_応募状況ボード.md


---

### B-10. next/font の変数クラスを `<body>` に付けて CSS 変数チェーンが全滅した (頻度: 中、最終発生: 2026-08-28) #tool

**状況**: ハタチたち公式 HP (Next.js + Tailwind) を実装。明朝体 (Noto Serif JP) を next/font で読み込み、`body` タグに font variable className を付けた。スクリーンショット上は「明朝っぽい」雰囲気のまま、22 エージェントの全面レビューで `getComputedStyle` を実測するまで発覚しなかった。

**過去のやらかし**:
- 2026-08-28: `<body className={`${notoSerifJP.variable} ...`}>` という実装をしたため、`:root` から CSS 変数 `--font-serif` が解決できず、全ページで明朝体が一文字も適用されなかった。代わりにシステムのゴシック体が描画されていた。同時に、Japanese フォントの preload が head に 123 本 (約 3.8MB 相当) 入っていたのも see ともに未検出。レビュー後、変数クラスを `<html>` に移動して 1 本に絞り解消。

**正しい挙動**:
- next/font の `variable` モードは `<html>` タグ (layout.tsx の最外殻) に付ける。`<body>` ではなく `<html>` に付けることで `:root` から CSS 変数が解決される
- 実装後は `getComputedStyle(document.body).fontFamily` をブラウザで実測して期待のフォント名が出るか確認する
- スクリーンショットだけでは「それっぽく見える」ため、フォント適用の検証はスクショ目視だけで済ませない

**再発防止**:
- next/font 実装時のチェック: `<html>` に variable className を付ける → ビルド後にブラウザコンソールで `getComputedStyle` 実測 → フォント名が出ない場合はクラスの付け先を疑う
- preload が大量に入っている兆候 (build ログの Preloaded resources が多い) も見る。`subsets: ['japanese']` 指定でも weight ごとに file が生成されるため、weight を 1-2 本に絞る

---

### B-9. 画像PDFが読めなかったのを理由に、施設の客席構成を推定で断定した (頻度: 低、最終発生: 2026-08-27) #hallucination

**状況**: 久喜総合文化会館 大ホールでの舞台収録レンズ選定。YDがホールのURLを貼って質問。カメラ〜舞台の距離を出すのに客席の階構成が必要だったが、座席表PDFが画像PDFで WebFetch がテキストを抽出できなかった。

**過去のやらかし**:
- 2026-08-27: 「1,218席規模のプロセニアムホールの一般値」から「1階最後列25〜28m / 2階最前列28〜30m / 2階後方34〜36m」と書き、2階席が存在する前提で花道込みの構図設計まで組み立てた。**このホールに2階席は無い (1階のみ全29列)**。YDに「2階席ないわ!! ちゃんと送ったリンク確認して!!」と指摘されて発覚。推定であることは書き添えていたが、存在しないフロアを前提に具体的な数値と構図案を出した時点で実害は同じ。

**正しい挙動**:
- 画像PDFで中身が読めない時は「読めないから推定する」ではなく、**画像化して見る**。Mac側で `curl -sL -o /tmp/x.pdf <url>` → `sips -s format png -Z 2400 /tmp/x.pdf --out /tmp/x.png` → Desktop Commander の `read_multiple_files` で画像として読ませれば中身が見える (今回これで5分で確定した)
- 席数・規模から階構成を推測しない。座席表は必ず実物を見る
- 一般値からの推定を出す場合でも、**存在するかどうか自体が未確認の対象 (2階席) を前提にした設計は書かない**。推定してよいのは「実在が確認できたものの寸法」まで

**再発防止**:
- 施設・会場の質問では、YDが貼ったURLの配下にあるダウンロード資料を全部当たり、読めない形式は画像化してでも実見してから数値を出す
- 「〜規模のホールの一般値では」という書き出しが出そうになったら、その前に一次資料を見きったか自問する
- 関連: [[#B-8]] (未知の固有名詞を推測で断定)、[[#2026-08-24 エントリー受付中を実選考と誤認]] と同根 = **確認可能なものを確認せずに推定で埋める**

---

### A-24. nohup レンダーの進捗出力を捨てて残り時間が追跡不能になった (頻度: 低、最終発生: 2026-08-29) #tool

**状況**: Remotion の ProRes 4444 書き出し (`nohup npx remotion render ...`) で進捗を確認しようとしたところ、出力をパイプで捨てていたため残り時間が出せなかった。YD から「後どれくらい?」と問われ、CPU 使用率と 1080p版の実績秒数から「10〜20分」と推定するしかなかった。

**過去のやらかし**:
- 2026-08-29: プペルエンドロール 4K ProRes 4444 レンダー。nohup で起動したが stdout/stderr の誘導先を指定せず、Remotion の進捗表示が取得できない状態で走らせた。30 分経過後に YD から進捗を聞かれ、間接証拠 (Chrome CPU 100%・1080p レンダーの 6 分実績 × 4倍) で「あと 10〜20 分」と推定した。結局 30GB が大きすぎるとして中断・H.264 に切り替え。

**正しい挙動**:
- バックグラウンドレンダーを起動するときは必ず `nohup ... > /tmp/render.log 2>&1 &` のようにログファイルへ誘導し、`tail -f /tmp/render.log` でいつでも進捗を確認できる状態にしておく
- Remotion は `--log=verbose` オプションで進捗率・残りフレーム数を出す。ログファイルと組み合わせると `grep "%" /tmp/render.log | tail -1` で最新進捗が取れる

**再発防止**:
- bg レンダー起動時のテンプレ: `nohup npx remotion render ... > /tmp/remotion_render.log 2>&1 & echo "PID=$! log=/tmp/remotion_render.log"`
- 「後どれくらい?」という質問が来た時点で log ファイルが無ければ即座に「進捗を取れる形で再起動すべきか?」を YD に確認する

## 2026-09-04 ffmpeg の計測コマンドに `-v error` を付けて何も返らないと誤認しかけた

プペル琴の音量計測で `ffmpeg -v error -i F -af volumedetect -f null -` を回して、出力が空だったので「測れない」と一度判断しかけた。volumedetect / loudnorm の print_format / ebur128 の結果は info レベルで出るので、`-v error` を付けると全部消える。

**再発防止**: ffmpeg の解析系フィルタは `-hide_banner` だけにする。`-v error` も `-nostats` も付けない。出力が空だった時は「データが無い」ではなくまずログレベルを疑う。

## 2026-09-04 「音が小さい」の指摘に対して、測る前に対処案を並べた

YD の「なんかめっちゃ音小さくない?」に対して、測定前に「そのまま / −14 LUFS に正規化 / iPhone に合わせる」の3択を出した。実際に測ったら −11.3 LUFS で配信基準より大きく、上げ代は 1.4dB しか無かった。選択肢のうち2つは最初から成立していなかった。

**再発防止**: 数値で確認できることを聞かれたら、選択肢を出す前に測る。ソースファイルが手元に無くても、納品済みならギガファイルや Drive から回収して測れる。

## 2026-09-05 Tailwind の `md:` でスマホ専用スタイルを上書きしようとした

縦書きのモバイル用 `leading-[1.15]` を当てる際に `md:leading-normal` と書いた。これは「md以上でleading-normal (1.5)」なのでデスクトップを上書きする。正しくは `max-md:leading-[1.15]`（md未満のみ）。開発サーバーで実測して発見・自己修正した（デスクトップの継承値1.9が1.5になるところだった）。

**再発防止**: Tailwind でスマホ専用スタイルを書くときは `max-md:` / `max-sm:` を使う。`md:` 以上プレフィックスは「md以上に適用」なのでデスクトップを巻き込む。変更後は必ずデスクトップ幅でも計測確認する。
