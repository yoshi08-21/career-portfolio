# 引き継ぎ資料（career-portfolio セッション）

> 最終更新: 2026-06-28（Slack deny 動作テスト **クローズ** → §4-A2。結論: deny 対象 mutating 8種は **deny 設定により surface（ツール一覧表示）自体が抑止され呼び出し不能**。別セッションでも非surfaceを再確認し、「deny 無→surface／deny 有→非surface」の対照で判断確定。当初の「実弾でブロック観測」は対象不在につき不要。）
> セッションは **`~/Applications/development/`（横断ルート）で開く**。このファイルを `@career-portfolio/handover.md` でメンションすれば現状を把握できる。
> 併せて読む: `@career-portfolio/CLAUDE.md`（方針）→ `@career-portfolio/ideas.md`（強化アイデア／ロードマップ）→ `@career-portfolio/README.md`（構成）。

---

## 0. 現在の状態（コミット・同期）

- ⚠️ **working tree に未コミットの変更あり（commit はご指示待ち）**。前セッションのインジェクション対策追記＋今セッションの Slack 連携実装が未コミット。新セッション開始時に `git status` で確認。**勝手に commit しない**。未コミットの内訳:
  - M `CLAUDE.md`（取り込みコンテンツの安全な扱い 追加）
  - M `tools/shared/slack-mcp-setup.md`（インジェクション注意・トークン失効/更新セクション・xincere 登録例・サーバ⇄会社マッピング表）
  - M `tools/b1-auto-inventory/playbook.md`（S2-2.5 Slack コンテキスト補強・3層保存動線・TODO 参照・サーバ選択 1対多）
  - M `tools/b1-auto-inventory/README.md`（Slack 保存層の表・TODO 注記）
  - M `tools/b1-auto-inventory/templates/source-deepdive.md`（Slack 由来の反映欄）
  - ?? `tools/b1-auto-inventory/templates/slack-threads.md`（①索引テンプレ・新規）
  - ?? `tools/b1-auto-inventory/templates/slack-archive.md`（②アーカイブテンプレ・新規）
  - ?? `tools/b1-auto-inventory/TODO-slack-dryrun.md`（B 実走の検証/改善 TODO・新規）
- 直近の主なコミット（push 済み）:
  - `docs: Slack MCP 接続手順を追加し handover を最新化`
  - `sources: supernova 棚卸し台帳`（B1 試運転）/ `tools: B1 自動棚卸しツール` / `ideas: 強化アイデア記録ファイル` / `appeals/sources に会社ディレクトリ追加`
- ⚠️ **git 運用ルール（厳守）**: `git add`/`commit`/`push` は**本人の明示指示があるまで実行しない**。指示で commit した場合も **push 前に必ず確認**。

---

## 1. このリポジトリは何か（3行）

業務経験を「経験 → 整理 → アピール → レジュメ転記」へつなぐ個人リポジトリ。複数プロジェクト（在籍企業）を横断するハブで、対象リポジトリをメンションして整理を依頼する。**当事者性と定量を主役**にし、事実は入力から取り数値は創作しない。
→ 現在、これを **「転職活動全般を支える伴走ツール」へ拡張中**（下記 §2・`ideas.md`）。

---

## 2. このセッションでやったこと（時系列・理由つき）

1. **新プロジェクトを `development/` へ移動した状態を確認**
   - `development/` 直下に `career-portfolio` と並んで: `supernova` / `duoduo-nextjs` / `naminori` / `simount`(中に logi-go・pms 系) / `upbond`(中に botejyu・wako 系) / `xincere-blog-dir`。
   - remote org → 会社ディレクトリ対応: `xincere-inc→xincere`（命名は `xincere` に統一）, `simount`, `upbond`, `SupernovaInc→supernova`。
   - `appeals/<会社>/`・`sources/<会社>/` に `xincere`/`simount`/`upbond` を追加（`.gitkeep`）。

2. **`ideas.md` を新設（強化アイデア／ロードマップ）**
   - ビジョン: 経験整理ハブ → 転職活動の伴走ツール／**役割の違うサブエージェント同士のディスカッションで精度を上げる**。
   - サポートしたい9領域（強み発見・面接回答・自己PR・職経/レジュメのブラッシュ・やりたいこと言語化・企業選び/受験判定・躓きサポート 等）を北極星として明記。
   - Backlog **B1〜B18**（フェーズ／対応ゴール／使うサブエージェント／優先度・状態）。番号は順序でなくインデックス。使い方の流れ（シナリオ例）も記載。
   - サブエージェント役割カタログ8種＋討議パターン3種。

3. **`tools/` の土台を確立**（全ツール共通）
   - 方針: **メンション駆動の自己完結 Markdown プレイブック**（`.claude/` 非依存。横断ルートで開く運用に合わせる）。
   - 構成ルール: **1ツール=1ディレクトリ**、命名 `b<NN>-<slug>`（`ideas.md` の B番号と一致）。**定義（tools/）とデータ（appeals/・sources/）を分離**。共有部品は `tools/shared/`。

4. **B1「git履歴/PR からの自動棚卸し」を実装＋実リポジトリで試運転**（`tools/b1-auto-inventory/`）
   - Stage 1（sweep）: `gh pr list --author=@me` で PR を機能単位に抽出 → 暫定スコア → 台帳 `sources/<会社>/_inventory.md`。Stage 2（深掘り）: PR/差分から自動ドラフト → インタビュアー役が質問生成・検証役が裏付けチェック → `sources/<会社>/<テーマ>.md`。
   - 原則: **git は読むだけ・無い数値はプレースホルダー・B1 は sources まで（appeal は作らない）**。
   - 試運転: `supernova` の `stella-ai-biz-backend` / `stella-tmp-organization-admin`、直近3ヶ月で **8タスク候補**を抽出し台帳化（`sources/supernova/_inventory.md`）。学び（リポジトリ列・機能テーマでの束ね）を playbook/テンプレへ反映済み。
   - 共有エージェント: `tools/shared/agents/interviewer.md`・`verifier.md`。

5. **Slack 連携の準備（B1 強化）**: `slack-supernova` MCP を接続
   - 棚卸しに Slack の議論・判断の流れを取り込むため。この環境に Slack/Linear の MCP は元々無く、調査の上 `korotovsky/slack-mcp-server` をブラウザトークン方式で接続。
   - **読み取り専用で運用**（`SLACK_MCP_ADD_MESSAGE_TOOL` は付けない＝投稿ツール自体がロードされない。`=false` も付けない＝仕様外で誤登録の恐れ）。手順は `tools/shared/slack-mcp-setup.md`。
   - **ワークスペースは会社ごとに別**。`xoxc` は team 単位のため **1サーバ=1ワークスペース**。SUPERNOVA 用に `slack-supernova` を登録済み。別ワークスペースは `slack-<名前>` で追加する。
   - 会社ポリシー上 Slack 連携の禁止は無いことを本人が確認済み。

6. **【今セッション】Slack 連携を B1 に実装＋ xincere ワークスペース追加**（手順面は一通り完成・実走は未）
   - **Slack 読み取り疎通テスト済み**: `slack-supernova` で `channels_me`・`STELLA` 検索が通り、PR URL/チケットID/本人発言がヒットすることを確認。投稿ツールが無い（＝読み取り専用）ことも確認。
   - **B1 Stage 2 に `S2-2.5 Slack コンテキスト補強` を実装**（playbook）。本人提案の2段構え: ①自分起点で探す（チケットID/PR URL/`filter_users_from=本人`/メンション）→ ②`conversations_replies` でスレッド全体を読み流れを把握。
   - **3層保存モデルを設計・実装**:
     - ① 索引(dedup): `sources/<会社>/_slack-threads.local.md`（座標 channel+ts＋1行要点＋状態。Git 追跡外）。同じスレッドの二重読み込み防止。再確認可。
     - ② アーカイブ: `private/slack/<会社>/<テーマ>.md`（中精度・マスク済み。**自分の発言=原文寄り／他者=議論の流れが失われない程度の要約**。Git 追跡外＝`private/` は gitignore 済）。
     - ③ 素材: `sources/<会社>/<テーマ>.md`（抽象化済み・appeal 元ネタ。Git 追跡）。
   - テンプレート追加: `templates/slack-threads.md`(①)・`templates/slack-archive.md`(②)。`source-deepdive.md`(③) に Slack 反映欄を追記。
   - **`TODO-slack-dryrun.md` を新設**: B 実走時の検証項目・改善観点チェックリスト。playbook S2-2.5/README に「初回実走時は必ず TODO を実施」と明記（実施忘れ防止）。
   - **`slack-mcp-setup.md` を増補**: 「トークンの失効と更新」独立セクション／xincere 登録コマンド例／**Slack サーバ⇄会社ディレクトリ マッピング表**。
   - **`slack-xincere` MCP を接続**（本人がターミナルで登録・`claude mcp list` で Connected 確認済み）。**1ワークスペースが複数会社に対応**: `slack-xincere` = SUPERNOVA 以外の全開発（`xincere`/`simount`/`upbond`）。**ただし新セッション再起動前のため、このセッションでは `slack-xincere` ツール未ロード**。

7. **【今セッション】Slack の書き込み系ツールを `deny` 設定でブロック**（安全強化）
   - 経緯: 「読み取り専用」と認識していたが、ロード済みツールを精査したところ **メッセージ投稿ツールは無い（＝なりすまし投稿の経路は無い）が、状態を変える mutating ツールは存在**することが判明（`usergroups_create/update/users_update`・`conversations_join/leave/mark`・`saved_update/clear_completed` の8種）。
   - 対策: `~/.claude/settings.json`（**ユーザー全体**。Slack 状態変更はどのプロジェクトでも使わない想定のため）の `permissions.deny` に、**両サーバ分 計16ルール**（`slack-supernova`×8＋`slack-xincere`×8）を登録。`jq` で16件パースを検証済み。
   - 効く理由: deny は**モデルの自制ではなくハーネスが実行直前に機械的に拒否**し、`deny > ask > allow` で最優先。ただし「ロードから外す（スキーマ非表示）」ではなく「呼んでも実行されない」形。
   - **検証済み（→ §4-A2 でクローズ）**: 個別ツール名（`mcp__server__tool`）粒度の deny は効く。実観測した挙動は「呼んでも拒否」より強く、**deny 対象ツールはツール一覧に surface すらしない**（deny 無→surface／deny 有→非surface の対照で確定）。settings は**セッション開始時に読まれる**ため、効くのは deny 保存後に開始したセッションから。

---

## 3. 現在の構成（ファイル地図）

```text
~/Applications/development/                 ← 横断ルート（ここで開く）
├── career-portfolio/                       ← このリポジトリ
│   ├── CLAUDE.md / README.md / START-HERE.md / handover.md
│   ├── ideas.md                            ← 強化アイデア（ロードマップ）
│   ├── templates/  (appeal/resume/request/selection-request)
│   ├── tools/                              ← ツール定義（定義のみ・データは置かない）
│   │   ├── README.md                       ← ツール一覧・命名規約・呼び出し方
│   │   ├── shared/
│   │   │   ├── agents/ (interviewer.md, verifier.md)
│   │   │   └── slack-mcp-setup.md          ← Slack MCP 接続/更新手順・サーバ⇄会社マッピング
│   │   └── b1-auto-inventory/
│   │       ├── README.md / playbook.md
│   │       ├── TODO-slack-dryrun.md         ← B 実走の検証/改善 TODO（次にやる）
│   │       └── templates/ (inventory, source-deepdive, slack-threads①, slack-archive②)
│   ├── appeals/<会社>/  (supernova に既存3件 / xincere・simount・upbond は空)
│   └── sources/<会社>/  (supernova/_inventory.md=B1台帳 / 各社 .gitkeep)
│         └─ ※実走時に生成（Git 追跡外）: _slack-threads.local.md①
├── private/                                ← Git 追跡外（gitignore）。②実走時に slack/<会社>/<テーマ>.md
├── supernova/ · duoduo-nextjs/ · naminori/ · simount/ · upbond/ · xincere-blog-dir/
```
※ Slack サーバ⇄会社: `slack-supernova`→`supernova` / `slack-xincere`→`xincere`・`simount`・`upbond`（SUPERNOVA 以外の全開発）

---

## 4. これからやること

### A. まず確認: 新セッションで Slack ツール（両ワークスペース）がロードされたか
1. **このセッションの再起動が必要だったのは `slack-xincere` 追加後だから**（登録済みだが旧セッションには未ロード）。新セッションで `slack-supernova` と **`slack-xincere` の両方**のツールが見えるか確認。両方とも投稿ツールが無い（＝読み取り専用）ことも確認。
2. `slack-xincere` の軽い疎通テスト（`channels_me` 等）。supernova 側は前セッションで疎通済み。

### A2. Slack deny 設定の動作確認 — ✅**クローズ済み（2026-06-28）**: deny は surface 自体を抑止＝mutating ツールは呼び出し不能
> §2-7 で入れた `permissions.deny`（計16ルール）が**個別ツール粒度で実際に効くか**を確かめる目的。当初は「`conversations_mark` を1回呼んでブロックを観測」する計画だったが、**そもそも mutating ツールがツール一覧に出ない**ため、より強い形（呼ぶ対象が存在しない）で安全が確認できた。

> **【結論 2026-06-28】deny 対象 mutating 8種は、deny 設定により surface（モデルへのツール提示）自体が抑止される。**
> - **決め手＝対照**:
>   - deny **無し**のセッション（このファイルへ deny を追記する前に開始したセッション）→ 開始時 deferred 一覧に **mutating 8種×2サーバ＝16ツールが全部 surface していた**。
>   - deny **有り**のセッション（deny 保存後に開始。複数セッションで再確認）→ **mutating ツールは ToolSearch にヒットせず、一覧にも出ない**。
>   - 変わった条件は **deny の有無だけ** → 「(a) deny が surface を抑止」で確定。「(b) ゲートウェイが元々 read 系しか出さない」は、deny 無しで surface した事実により**否定された**。
> - 利用可能な slack ツールは**両サーバとも読み取り専用のみ**（`channels_*`/`conversations_history`/`conversations_replies`/`conversations_search_messages`/`conversations_unreads`/`saved_list`/`usergroups_list`/`usergroups_me`/`users_search`）。
> - `settings.json` の `permissions.deny` は**16ルール（両サーバ×8）が正しく存在**（`jq` 確認済み）。**mutating ツールは一切呼んでいない**（呼べない）。
> - 残る厳密な留保（実用上は無視可）: 100% 詰めるなら deny を一時的に外して再出現を見ればよいが、**mutating ツールを再露出させる必要があり非推奨**。上の対照で十分。
> - 👉 **このテストはクローズ。再確認・実弾テストとも不要**。

#### （参考・当初のテスト計画。将来 mutating ツールが surface する環境に遭遇した場合のみ使う）

- **対象ツール**: `conversations_mark`（deny 対象8種のうち**最も副作用が小さい**＝自分の既読位置が動くだけ。グループ作成や参加/退出のような他者に見える変化は無い）。
- **期待結果**: `deny` ルールにより**ハーネスがブロックして実行されない**こと（拒否メッセージが返る）。実際に既読位置が動いてしまったら deny が効いていない＝設定の粒度・反映を見直す。
- **【厳守】テスト手順 — ツール実行前に必ず説明＋本人承認を待つ**:
  1. AI は `conversations_mark` を呼ぶ**前に**、(a) どのサーバ（`slack-supernova` か `slack-xincere`）の (b) どのツールを (c) どんな引数（channel / ts 等）で呼ぶつもりかを**日本語で具体的に説明**する。
  2. 説明後、**本人の明示的な承認（「OK」「実行して」等）を待つ**。承認があるまで**絶対にツールを呼ばない**。
  3. 承認を得て初めて1回だけ呼び、結果（ブロックされたか／実行されたか）を報告する。
  4. このテストは1回で十分。複数回・他の deny 対象ツール（特に他者に見える `usergroups_*`・`conversations_join/leave`）では**テストしない**。

> ※この「実行前に説明＋承認待ち」の所作は、deny テストに限らず**状態を変えうる操作（書き込み・mutating・外部送信）全般**に適用する（[[§6 注意点]] の方針と整合）。

> 新セッションへの最初の一言（推奨）:
> 「`@career-portfolio/handover.md` を見て。Slack の deny 設定を入れたので、まず §4-A2 の `conversations_mark` deny 動作テストを（実行前に説明＋承認のうえ）1回やって。その後 B1（§4-B）の Slack 連携を実物で1本通す。」

### B. 最優先: B1 を end-to-end で1本通す（実走）★次にやる本丸
- **手順は実装済み（S2-2.5＋3層保存）だが、まだ一度も実走していない**。`tools/b1-auto-inventory/TODO-slack-dryrun.md` の検証項目・改善観点に沿って1本通す。
- 候補（台帳 `sources/supernova/_inventory.md` の ◎）: `#1928 service_start_date 導入` 単体、または「メンバー追加/削除オーダー 更新API」テーマ束ね（#1738/#1760/#136）。
- 実走で①②③が生成できるか・マスク品質・dedup・検索精度を検証し、**学びを playbook/テンプレに反映**（試運転後に「Slack 連携は試運転前」注記を外す）。
- 出力 `sources/supernova/<テーマ>.md`(③) を素材として既存 appeal 流れ（B6 等）へ渡す。

### C. 次のツール（`ideas.md` Backlog から）
- 候補: **B4 強み発掘インタビュー**（サブエージェント討議の核）/ **B15 模擬面接** など。各ツールは §4 の土台ルールに従い `tools/b<NN>-<slug>/` に作る。

---

## 5. 確立したルール（新セッションが従う要点）

- **ツールはメンション駆動のプレイブック**（`tools/<tool>/playbook.md` を `@` で呼ぶ）。`.claude/` に依存しない。
- **1ツール=1ディレクトリ**、命名 `b<NN>-<slug>`。**定義（tools/）とデータ（appeals/・sources/）を分離**。共有は `tools/shared/`。
- **サブエージェント討議で精度を上げる**（役割: interviewer/verifier/critic/採用担当/コーチ/企業分析/編集/ジャッジ。実現は Agent・Workflow）。
- **固有名**: 進行中は実名／レジュメ化時に一括抽象化（一般技術名 Go/Slack 等は抽象化しない）。
- **保存先**: appeal=`appeals/<会社>/<YYYY-MM-テーマ>.md`、素材=`sources/<会社>/`。会社=remote org 基準（xincere/simount/upbond/supernova）。
- **Slack は読み取り専用**・ワークスペースごとに `slack-<名前>` で別登録。**1ワークスペース→複数会社あり**: `slack-supernova`→supernova / `slack-xincere`→xincere・simount・upbond。棚卸し時はこの対応でサーバを選ぶ（`slack-mcp-setup.md` のマッピング表）。
- **Slack 由来は3層保存**: ①索引 `_slack-threads.local.md`（追跡外）／②アーカイブ `private/slack/...`（追跡外・自分=原文寄り/他者=流れを保つ要約）／③素材 `sources/<会社>/<テーマ>.md`（追跡）。原文寄りは②に限定、生文/permalink/実名は①③に書かない。
- **CLAUDE.md は自動ロードされない**（横断ルートで作業するため毎回 `@career-portfolio/CLAUDE.md` を明示）。

---

## 6. 注意点

- **git 操作は本人指示後のみ・push 前に確認**（個人 private・各社固有名を含むため特に慎重に）。
- **状態を変えうる操作は「実行前に説明＋本人承認待ち」**（書き込み・mutating・外部送信・Bash・git add/commit/push 等）。AI は実行する前に「どのツール/コマンドを・どんな引数で・何のために」呼ぶかを具体的に説明し、**本人の明示承認を得てから**実行する。Slack 系では特に、deny 設定済みの mutating ツール（`conversations_mark` 等）を**テスト目的でも**呼ぶ際は §4-A2 の手順（説明→承認→1回のみ）に従う。読み取り専用ツール（`channels_me`・`conversations_search_messages`・`conversations_replies` 等）は通常どおり説明のうえ進めてよい。
- **Slack トークンは失効しうる**（`d` Cookie のセッション切れ：ログアウト/セッションポリシー/パスワード変更等）。`invalid_auth`/`not_authed` が出たら再取得→`claude mcp remove`→`add` 再登録→セッション再起動（**専用セクション「トークンの失効と更新」が `slack-mcp-setup.md` にある**）。
- **Slack 取り込み時の機密配慮**: 生スレッド・他者名をそのまま追跡対象（①③）に書かない。原文寄りは②`private/`（追跡外）に限定。自分の判断・当事者性の要点を抽出。秘密情報（トークン/鍵/`.env`）は②でも書かない。
- 既存ドラフト `appeals/supernova/` の3件は下書き（数値・固有名にプレースホルダー）。実データで仕上げる。
