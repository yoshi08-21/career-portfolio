# Slack MCP 接続セットアップ

棚卸し(B1)などで Slack のスレッド/自分の発言を素材に使うため、Claude Code に Slack MCP サーバを接続する手順。
共有部品（複数ツールから利用）として `tools/shared/` に置く。

- 採用サーバ: [korotovsky/slack-mcp-server](https://github.com/korotovsky/slack-mcp-server)
- 方式: **ブラウザトークン（xoxc / xoxd）**＝管理者の App 承認不要で、**自分が見えている範囲**を読める

---

## ⚠️ 始める前に（必読）

1. **会社のSlackポリシー確認**: ブラウザのセッショントークンを取り出して外部ツールに渡す行為は、会社のセキュリティ規定で禁止されている場合がある。SUPERNOVA の規定上問題ないか確認すること（後述「正規の手順か？」も参照）。
2. **トークンの扱い**: `xoxc`/`xoxd` は「自分として全部読める」強力な資格情報。**チャットに貼らず、自分のターミナルで直接コマンドを実行**する（会話記録にトークンを残さない）。
3. **読み取り専用で使う**: 投稿系ツールを有効化する `SLACK_MCP_ADD_MESSAGE_TOOL` は**設定しない**（棚卸しは読むだけ）。

---

## Step 1. ブラウザから2つのトークンを取得

`app.slack.com` にログインした状態で行う。

### ① `xoxc-…`（SLACK_MCP_XOXC_TOKEN）= Web クライアントの API トークン
1. 開発者ツール → **Console** タブ
2. `allow pasting` と入力して Enter（貼り付け許可）
3. 次を実行:
   ```js
   JSON.parse(localStorage.localConfig_v2).teams[document.location.pathname.match(/^\/client\/([A-Z0-9]+)/)[1]].token
   ```
4. 出力された `xoxc-…` をコピー

### ② `xoxd-…`（SLACK_MCP_XOXD_TOKEN）= セッション認証 Cookie `d`
1. **Application** タブ → 左の **Cookies**
2. `d` という名前の Cookie を探す
3. Value をダブルクリックして全選択 → コピー（`xoxd-…`）

> ②の `d` Cookie と ①の `xoxc` は**セットで初めて認証が通る**（xoxc 単体では不可）。詳しくは末尾「Step 1 は何をしているか」。

## Step 2. 自分のターミナルで MCP を登録

> ⚠️ AI ではなく**自分のターミナル**で実行（トークンを会話に残さない）。`<...>` を置換。

```bash
claude mcp add slack -s user \
  -e SLACK_MCP_XOXC_TOKEN="<xoxc-で始まるトークン>" \
  -e SLACK_MCP_XOXD_TOKEN="<xoxd-で始まるトークン>" \
  -- npx -y slack-mcp-server@latest --transport stdio
```

- `-s user` = 全プロジェクトで使える（横断ルート運用と相性◎）。このリポジトリ限定なら `-s local`。

### 読み取り専用の担保（メッセージを送らせない）

「Claude Code 経由で Slack にメッセージを送らない」を**仕組みで**保証する。

1. **投稿ツールを出さない（必須）**: 投稿系ツール（`conversations_add_message` 等）は**デフォルトで未登録**。`SLACK_MCP_ADD_MESSAGE_TOOL` を設定したときのみ有効化される。**この環境変数を付けない**＝投稿ツールがそもそもロードされず、AI の手元に送信手段が存在しない（＝物理的に送れない）。上記コマンドは付けていないので既に読み取り専用。
   - ⚠️ **`=false` を明示的に付けてはいけない**。この変数は boolean ではなく「有効化の指定」（`true` / チャンネルIDのカンマ区切り / `!ID` で除外）。`false` は仕様外の値で、最悪「`false` というIDのホワイトリスト」と解釈され**投稿ツールを登録してしまう**恐れがある。**変数自体を書かない**のが確実。
   - `SLACK_MCP_ENABLED_TOOLS` に書き込み系ツールを列挙しないこと（列挙すると上記が空でも登録され得る）。
2. **トークンに投稿権限を持たせない（推奨・二段目）**: 正規 `xoxp` 方式で `chat:write` 等の書き込みスコープを付けなければ、仮に投稿ツールがあっても **Slack 側が拒否**する（資格情報レベルで不可）。
   - 注意: ブラウザトークン（xoxc/xoxd）は「自分のセッション全権」なので投稿能力自体は持つ。この方式では担保は 1.（投稿ツールを出さない）に依存する。

→ 厳密に送信を排除したいなら **「1. 投稿ツールを出さない」＋「2. xoxp を読み取りスコープのみで発行」** の二段構えにする。

### 複数ワークスペースの扱い（重要）

`xoxc` トークンは**ワークスペース（team）ごとに別物**。1つの MCP サーバには1セットのトークンしか渡せない＝**1サーバ＝1ワークスペース**。
複数ワークスペース（例: SUPERNOVA 用と、それ以外のアプリ開発用）を使うなら、**ワークスペースごとに別名で MCP を登録**する。

- **命名はワークスペース／会社で区別**する（career-portfolio の会社ディレクトリと揃える）: `slack-supernova`, `slack-<別ワークスペース名>` …。`slack` という汎用名は使わない（後で衝突・混乱するため）。
- 例（SUPERNOVA 用・今回取得済みのトークンで登録。自分のターミナルで実行）:
  ```bash
  claude mcp add slack-supernova -s user \
    -e SLACK_MCP_XOXC_TOKEN="<SUPERNOVA の xoxc>" \
    -e SLACK_MCP_XOXD_TOKEN="<d Cookie の xoxd>" \
    -- npx -y slack-mcp-server@latest --transport stdio
  ```
- **別ワークスペースを足すとき**: そのワークスペースを**ブラウザで開いた状態**で Step 1 のコンソールコマンドを再実行して**そのワークスペースの xoxc** を取得（`xoxd` の `d` Cookie は同じブラウザログインなら共通のことが多い）→ `slack-<別名>` で同様に登録。
- 棚卸し時は「対象リポジトリの会社 → 対応する slack サーバ」を選んで使う（例: `supernova/*` の棚卸し → `slack-supernova`）。

## Step 3. 反映と確認

```bash
claude mcp list      # slack が出ればOK
```
この後 **セッションを開き直す**と Slack ツールがロードされる（初回は npx の取得に少し時間がかかる）。

---

## 補足

- **トークンは失効する**: `d` Cookie はセッション切れで無効化される。Slack を再ログインしたら再取得→再登録が要る場合がある。
- **第三者製 MCP**: `npx ...@latest` は npm 最新版を都度取得。気になればバージョン固定（`slack-mcp-server@<version>`）。
- 接続後、B1 Stage 2 に「コンテキスト補強」ステップ（PR↔Linear↔Slack の紐づけ・要点のみ素材化・機密配慮）を実装する。

---

## Step 1 は何をしているか

- **xoxc トークン**: Slack の Web アプリ自身が API 呼び出しに使っている**ワークスペース用トークン**。ブラウザの `localStorage`（Slack が保存している `localConfig_v2`）から、今開いているワークスペースの分を読み出している。
- **xoxd Cookie（`d`）**: ログイン状態を証明する**セッション Cookie**。Slack の Web API は「xoxc トークン＋`d` Cookie」の**両方**が揃って初めて本人として認証される。
- つまり Step 1 は、**ブラウザのログインセッションそのものの資格情報を取り出して、プログラム（MCP サーバ）が"ログイン中の自分の Web セッションと同じように"Slack API を叩けるようにしている**。新たに権限を発行しているのではなく、既存のブラウザセッションを借用している。

## これは正規（公式）の手順か？

**いいえ、Slack 公式が用意・推奨する方法ではない。** 以下を理解した上で使う。

- これは Slack の**内部 Web API をブラウザのセッション資格情報で叩く**非公式（コミュニティ）手法。公式の OAuth でアプリを発行する流れではない。
- **規約・ポリシー上の懸念**: Slack の利用規約（自動化・非公開APIの利用）や、会社のセキュリティ規定に抵触する可能性がある。**会社での利用可否は必ず自分で確認**する。
- 失効しやすく（ログアウトで無効化）、ワークスペース側で検知・遮断され得る。

### 正規の代替（公式 OAuth）== 推奨

管理者承認が得られるなら、こちらが本来の方法。棚卸しは「自分として読む」用途なので **ユーザートークン `xoxp-…`** を使う（ボット `xoxb-` は検索不可など制約が大きい）。

#### 手順（ユーザートークン `xoxp-…` を取得）

1. **Slack App を作る**: [api.slack.com/apps](https://api.slack.com/apps) → **Create New App** → **From scratch** → アプリ名（例 `career-portfolio-reader`）と対象ワークスペースを選ぶ。
2. **読み取りスコープを付与**: 左メニュー **OAuth & Permissions** → **Scopes → User Token Scopes** に、棚卸しに必要な**読み取りのみ**を追加（書き込みは付けない）:
   - `channels:history`, `channels:read` … パブリックチャンネルの履歴・一覧
   - `groups:history`, `groups:read` … プライベートチャンネル
   - `im:history`, `im:read` … DM
   - `mpim:history`, `mpim:read` … グループDM
   - `users:read` … 発言者名の解決
   - `search:read` … メッセージ検索（チケットID/PR URL でスレッドを探すのに重要）
   > 投稿・管理系（`chat:write` / `*:write` / `channels:manage` 等）は棚卸しでは不要なので**付けない**（最小権限）。
3. **ワークスペースにインストール**: 同ページ上部 **Install to Workspace** → 権限確認 → 許可。
   - 会社ワークスペースが App インストールを制限している場合、ここで **「管理者に承認をリクエスト」** する画面になる。出たら**管理者に申請**する（理由: 自分の業務棚卸し用の読み取り専用アプリ、と添えると通りやすい）。
4. **トークンをコピー**: インストール後、**OAuth & Permissions** に表示される **User OAuth Token（`xoxp-…`）** をコピー。

#### 登録（Step 2 の xoxp 版）

```bash
claude mcp add slack -s user \
  -e SLACK_MCP_XOXP_TOKEN="<xoxp-で始まるトークン>" \
  -- npx -y slack-mcp-server@latest --transport stdio
```

> `xoxp` 方式は xoxc/xoxd 不要（この1本でOK）。確認は Step 3 と同じ（`claude mcp list` → セッション再起動）。

#### ボットトークン `xoxb-…`（参考）
招待されたチャンネルのみ・`search.messages` 不可（=メッセージ検索ツールが使えない）。棚卸しには不向き。

> まとめ: **OAuth（xoxp）= 公式・正規・最小権限で安全。要 App 作成＋（多くは）管理者承認**。**ブラウザトークン（xoxc/xoxd）= 手軽・管理者不要だが非公式（グレー）**。会社ポリシーが許すなら **xoxp を推奨**。
