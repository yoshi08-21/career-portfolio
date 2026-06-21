# セッションの始め方（アピール文を作るとき）

別セッションでアピール文を作るときの**開始ルール**。この通りに始めれば、毎回同じ品質で `career-portfolio/appeals/` に下書きが出来上がる。

> **AIに渡すのは `CLAUDE.md`。START-HERE はあなた用のカンペ**（手順とコピペ用キックオフ文の置き場）。このファイル自体をメンションする必要はない。

## 前提：作業ルートは `supernova` か `supernova-biz`

3つのリポジトリは `supernova-biz/` 直下に並んでいる（兄弟ディレクトリ）。

```
supernova/                         ← ここで開くこともある
└── supernova-biz/                 ← 通常はここで開く（推奨）
    ├── career-portfolio/          ← アピール文の置き場（このリポジトリ）
    ├── sn-personal-notes/         ← タスク資料（例: tasks/stella-8272/）
    └── task-management/           ← 目標管理
```

**career-portfolio の中では作業しない**ため、`career-portfolio/CLAUDE.md` は自動では読み込まれない。→ **毎回、方針ファイルを明示的にメンションする**こと（下記ステップA）。

> 推奨は **`supernova-biz` で開く**こと。3リポジトリへの相対パスが短くなる。`supernova` で開く場合は各パスの先頭に `supernova-biz/` を足す。

## 1行でいうと

作業ルートで、**`@career-portfolio/CLAUDE.md`** と**①対象タスクの資料**・**②自分のメモ**を渡して「アピール文を作って」と頼む。

---

## ステップ

### A. 最初に方針を読ませる ＝【必須・自動ロードに頼らない】
- 通常は `supernova-biz` / `supernova` で作業するので、career-portfolio の `CLAUDE.md` は自動で効かない。
- だから**最初に必ず** `@career-portfolio/CLAUDE.md` をメンションする（任意で `@career-portfolio/START-HERE.md` も）。
  - `supernova` で開いた場合: `@supernova-biz/career-portfolio/CLAUDE.md`

### B. 何をメンション（@）するか ＝【必須2つ】
パスは**作業ルートからの相対**。

| | `supernova-biz` で開いた場合 | `supernova` で開いた場合 |
|---|---|---|
| ① 対象タスクの資料 | `@sn-personal-notes/tasks/<...>/` | `@supernova-biz/sn-personal-notes/tasks/<...>/` |
| ② 自分のメモ | `@career-portfolio/sources/<file>.md` | `@supernova-biz/career-portfolio/sources/<file>.md` |

- ①はPR / チケットのリンクでも可。
- ②はチャットに直接貼ってもよい。

> この2つ（＋A）さえあれば着手できる。足りない項目はAIがプレースホルダーで補い、「ここに実数を」と明示する。

### C. 何を別途共有するか ＝【あると精度が上がる】
- 用途（職務経歴書 / ポートフォリオ / 面接の口頭用）
- 応募職種・方向性（技術寄り / 業務貢献寄り / マネジメント）
- 定量成果（before→after の数値）※AIは数字を創作しないので、ここだけは実データ
- 伏せたい固有名（無ければ抽象化で）
- 特に推したいエピソード（気づき・判断の話）

→ `career-portfolio/templates/request-template.md` を埋めて貼るのが速い。

### D. 何をお願いするか ＝【依頼文】
> このタスクのアピール文を作って。`CLAUDE.md` の方針に沿って、**`career-portfolio/appeals/`** に `YYYY-MM-<テーマ>.md` で保存して。

---

## コピペ用キックオフ

### ▼ `supernova-biz` で作業する場合（推奨）

```
@career-portfolio/CLAUDE.md
@sn-personal-notes/tasks/<タスク>/      ← または PR/チケットのリンク

このタスクのアピール文を作ってください。CLAUDE.md の方針に沿って、career-portfolio/appeals/ に YYYY-MM-<テーマ>.md で保存してください。

- 私のメモ: <箇条書きを貼る、または @career-portfolio/sources/<file>.md>
- 用途: 職務経歴書 / ポートフォリオ / 面接口頭（どれか）
- 応募職種: <例: バックエンドエンジニア>
- 定量成果: <数字。無ければ「後で記入」>
- 伏せたい固有名: <無ければ「特になし＝抽象化で」>
- 特に推したい点: <あれば>
```

### ▼ `supernova` で作業する場合（パス先頭に `supernova-biz/` を付ける）

```
@supernova-biz/career-portfolio/CLAUDE.md
@supernova-biz/sn-personal-notes/tasks/<タスク>/

このタスクのアピール文を作ってください。CLAUDE.md の方針に沿って、supernova-biz/career-portfolio/appeals/ に YYYY-MM-<テーマ>.md で保存してください。
（以下、項目は上と同じ）
```

## 最低限ルート（メモが無くても始められる）

方針（A）と資料（①）だけメンションして「**ここからアピールできる経験を抽出して下書きを作って**」でも着手可能。AIが事実を拾って下書き化し、埋めるべき数値・確認したい点を明示する。

## 完成までの流れ

1. AIが `career-portfolio/appeals/` に下書きを作る（数値・固有名はプレースホルダー）。
2. **実データ**（達成率・グレード・テスト件数などの before→after、固有名の伏せ方）を埋める。
3. `add`/`commit`/`push` は本人の指示後に実行（push 前に確認）。
