# tools/ — career-portfolio のツール定義

career-portfolio を「転職活動の伴走ツール」にするための各機能（[[ideas.md]] の Backlog）の**定義**を置く場所。
ここには「**どう動くか（定義）**」だけを置き、「**成果物（データ）**」は置かない。データは `appeals/<会社>/`・`sources/<会社>/`（会社単位）に出力する。

## 呼び出し方（メンション駆動）

セッションは横断ルート `~/Applications/development/` で開くため、各ツールは `.claude/` に依存せず、**メンションして起動する自己完結の Markdown プレイブック**として作る。各ツールの `README.md` 冒頭の「起動文」をコピペして使う。

```
@career-portfolio/tools/<tool>/playbook.md に従って、<対象や条件> を実行して
```

## 構成ルール

- **1ツール = 1ディレクトリ**。命名は `b<NN>-<slug>`（`ideas.md` の B番号と一致させ、トレーサビリティを保つ）。
- 各ツールディレクトリの標準構成:
  - `README.md` … 概要・入出力・起動文・使い方
  - `playbook.md` … 実行手順の本体（エージェントが従う）
  - `templates/` … そのツール専用テンプレ（任意）
- **共有部品**は `tools/shared/` に置く:
  - `shared/agents/` … サブエージェントの役割定義（複数ツールから参照）
- **データは置かない**。出力先は `appeals/`・`sources/`・（将来）`applications/`・`interview/`。

## ツール一覧

| ツール | 概要 | 状態 |
|---|---|---|
| `b1-auto-inventory/` | git履歴/PR からの自動棚卸し（PR を機能単位に抽出 → 台帳 → 深掘りで `sources/` 素材化） | 実装着手 |

> 以降のツール（B2〜B18）は `ideas.md` の Backlog を参照。着手時にここへ追記する。
