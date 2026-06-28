# 引き継ぎ資料（career-portfolio セッション）

> 最終更新: 2026-06-28
> セッションは **`~/Applications/development/`（横断ルート）で開く**。このファイルを `@career-portfolio/handover.md` でメンションすれば現状を把握できる。
> 併せて読む: `@career-portfolio/CLAUDE.md`（方針）→ `@career-portfolio/ideas.md`（強化アイデア／ロードマップ）→ `@career-portfolio/README.md`（構成）。

---

## 0. 現在の状態（コミット・同期）

- working tree クリーン・`main...origin/main` 同期（push 済み）想定。新セッション開始時に `git status` で確認。
- 直近の主なコミット:
  - `slack-mcp-setup` 追加（Slack MCP 接続手順）
  - `sources: supernova 棚卸し台帳`（B1 試運転）
  - `tools: B1 自動棚卸しツール`（playbook/テンプレ/共有エージェント）
  - `ideas: 強化アイデア記録ファイル`
  - `appeals/sources に会社ディレクトリ追加（xincere/simount/upbond）`
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
│   │   │   └── slack-mcp-setup.md          ← Slack MCP 接続手順
│   │   └── b1-auto-inventory/ (README.md, playbook.md, templates/)
│   ├── appeals/<会社>/  (supernova に既存3件 / xincere・simount・upbond は空)
│   └── sources/<会社>/  (supernova/_inventory.md=B1台帳 / 各社 .gitkeep)
├── supernova/ · duoduo-nextjs/ · naminori/ · simount/ · upbond/ · xincere-blog-dir/
```

---

## 4. これからやること

### A. 最優先: 新セッションで Slack ツールをロード → 疎通 → B1 へ組み込み
1. **このセッションには Slack ツールが未ロード**（MCP 登録前から続くセッションのため）。**新セッションを開き直す**とロードされる。
2. 新セッションで `slack-supernova` のツールが見えるか確認 → **読み取り疎通テスト**（例: 参加中チャンネル一覧 / `STELLA-7279` 等のチケットIDでスレッド検索）。投稿ツールが出ていないこと（=読み取り専用）も確認。
3. **B1 Stage 2 に「コンテキスト補強」ステップを実装**: PR タイトルの `STELLA-XXXX`・PR URL をキーに Slack を検索 → 判断・議論の**要点だけ**を `sources/` に反映（**生ログは保存しない・機密配慮**）。Linear は少量のため手動貼り付けで対応。

> 新セッションへの最初の一言（推奨）:
> 「`@career-portfolio/handover.md` と `@career-portfolio/tools/shared/slack-mcp-setup.md` を見て。slack-supernova MCP 接続済み。B1（`tools/b1-auto-inventory/`）に Slack 連携を組み込む続き。」

### B. B1 を end-to-end で完成
- Stage 2（深掘り）を実リポジトリで1本通す。候補は台帳 `sources/supernova/_inventory.md` の ◎ や「機能テーマでの束ね」（例: メンバーオーダー更新機能 / service_start_date 導入）。
- 出力 `sources/supernova/<テーマ>.md` を素材として既存 appeal 流れ（B6 等）へ渡す。

### C. 次のツール（`ideas.md` Backlog から）
- 候補: **B4 強み発掘インタビュー**（サブエージェント討議の核）/ **B15 模擬面接** など。各ツールは §4 の土台ルールに従い `tools/b<NN>-<slug>/` に作る。

---

## 5. 確立したルール（新セッションが従う要点）

- **ツールはメンション駆動のプレイブック**（`tools/<tool>/playbook.md` を `@` で呼ぶ）。`.claude/` に依存しない。
- **1ツール=1ディレクトリ**、命名 `b<NN>-<slug>`。**定義（tools/）とデータ（appeals/・sources/）を分離**。共有は `tools/shared/`。
- **サブエージェント討議で精度を上げる**（役割: interviewer/verifier/critic/採用担当/コーチ/企業分析/編集/ジャッジ。実現は Agent・Workflow）。
- **固有名**: 進行中は実名／レジュメ化時に一括抽象化（一般技術名 Go/Slack 等は抽象化しない）。
- **保存先**: appeal=`appeals/<会社>/<YYYY-MM-テーマ>.md`、素材=`sources/<会社>/`。会社=remote org 基準（xincere/simount/upbond/supernova）。
- **Slack は読み取り専用**・ワークスペースごとに `slack-<名前>` で別登録。
- **CLAUDE.md は自動ロードされない**（横断ルートで作業するため毎回 `@career-portfolio/CLAUDE.md` を明示）。

---

## 6. 注意点

- **git 操作は本人指示後のみ・push 前に確認**（個人 private・各社固有名を含むため特に慎重に）。
- **Slack トークンは失効しうる**（`d` Cookie のセッション切れ）。読めなくなったら再取得→`claude mcp` 再登録（手順は `slack-mcp-setup.md`）。
- **Slack 取り込み時の機密配慮**: 生スレッド・他者名をそのまま `sources/` に書かない。自分の判断・当事者性の要点のみ抽出。
- 既存ドラフト `appeals/supernova/` の3件は下書き（数値・固有名にプレースホルダー）。実データで仕上げる。
