# 引き継ぎ資料（career-portfolio セッション）

> 最終更新: 2026-06-21
> 次セッションは **`~/Applications/development/`（横断ルート）で開く**。このファイルを `@career-portfolio/handover.md` でメンションすれば現状を把握できる。
> 併せて読む: `@career-portfolio/CLAUDE.md`（方針）→ `@career-portfolio/README.md`（構成）→ `@career-portfolio/START-HERE.md`（開始ルール）。

---

## 0. 🔴 最初にやること（未コミットの確定）

このセッションの **ディレクトリ移動・再配置・ドキュメント一般化が未コミット**で working tree に残っている。新セッションで最初に確定すること。

- **基準点（push 済み・リモート同期）**: `dcee535 c`
- **未コミットの差分**（`career-portfolio/` 内、`git status -s`）:
  - `M CLAUDE.md README.md START-HERE.md appeals/README.md sources/README.md`
  - `D appeals/2026-06-{ai-goal-management,delete-order-batch,order-csv-refactor}.md`（→ `appeals/supernova/` へ移動。**コミット時に rename 検出**）
  - `?? appeals/supernova/ sources/supernova/`（新規）
  - `?? handover.md`（このファイル）
- 推奨コミット:
  ```
  git add -A
  git commit -m "career-portfolio を横断ルートへ移動し、会社単位構成・入力ソース柔軟化に一般化"
  git push        # ※ push は本人確認の上で
  ```
- ⚠️ **git 運用ルール（厳守）**: `git add`/`commit`/`push` は**本人の明示指示があるまで実行しない**。指示で commit した場合も **push 前に必ず確認**。

> 補足: 「ディレクトリの物理移動」自体は git の差分には出ない（リポジトリが新しい場所に在るだけ）。git に出るのは上記の**内部再配置（rename）＋ドキュメント編集**のみ。

---

## 1. 現在地サマリ

- **配置**: `career-portfolio` を `…/supernova/supernova-biz/` 配下から **`~/Applications/development/career-portfolio`** へ移動済み（`supernova/` と兄弟）。独立 git リポジトリ・リモート `origin`（`git@github-personal:yoshi08-21/career-portfolio.git`）健全。
- **セッションを開く場所**: 今後は **`~/Applications/development/`**。ここからなら `@career-portfolio/...` と `@supernova/...` を短い相対パスでメンションできる。
- **位置づけ**: SUPERNOVA 専用 → **全プロジェクト横断のレジュメ作成ハブ**へ一般化済み。

```text
~/Applications/development/            ← 横断ルート（ここで開く）
├── career-portfolio/                  ← このリポジトリ
│   ├── CLAUDE.md / README.md / START-HERE.md / handover.md
│   ├── templates/  (appeal / resume / request / selection-request)
│   ├── appeals/<会社>/   ← supernova/ に既存3件
│   └── sources/<会社>/   ← supernova/（.gitkeep）
├── supernova/ …                       ← 対象プロジェクト（兄弟）
└── <これから移動してくる別プロジェクト>/
```

---

## 2. このリポジトリは何か（設計思想・3行）

業務経験を「**経験 → 整理 → アピール → レジュメ転記**」へつなぐ個人リポジトリ。対象プロジェクトをメンションして整理を依頼する横断ハブ。**当事者性（言われた外側で気づき・判断したこと）と具体性（定量）を主役**にし、事実は入力から取り数値は創作しない。

---

## 3. このセッションでやったこと（時系列・理由つき）

1. **delete-order-batch のプロジェクト経験を作成**（`appeals/supernova/2026-06-delete-order-batch.md`）
   - メンバー削除オーダーのバッチ自動削除（インシデント再発防止）。**進行中・仕様レビュー中だが仮仕様で先行実装済み**のタスク。
   - 記録方針は本人と相談し、**「仕様作成＋仮仕様での先行実装まで含めてアピール」**（承認後すぐPRに移れる状態を作った段取り判断を武器にする）を採用。

2. **固有名の「具体↔抽象」ライフサイクルをルール化**（`CLAUDE.md`）
   - 理由: 進行中タスクは追記のたびに事実照合が要るため**実名で書き、完了・レジュメ化時にまとめて抽象化**（最後の1パス）。実名ファイルは冒頭注記＋末尾の用語対応表を持つ。

3. **レジュメ完成版フォーマットを登録**（`templates/resume-template.md`）
   - 本人提供の実例に合わせ、**`課題/解決策/成果/関与度/使用技術` × 詳細版/圧縮版**を標準化。delete-order-batch にこの形式の「レジュメ完成版」を追記済み。

4. **アピールポイント取捨選択サポートを整備**（紙面の都合で1プロジェクト2〜3点に絞る支援）
   - **5軸ルーブリック**（当事者性・定量インパクト・技術難度・再現性・職種タグ）を `CLAUDE.md` に定義。
   - 各 appeal に **「アピールポイント候補（5軸評価）」表**を持たせる（`appeal-template.md` に反映、delete-order-batch に実例7点）。
   - **応募先ドリブンの選択**を回す `templates/selection-request-template.md` を新設（ハイブリッド方式＝土台は共通・最終選択だけ応募先で出し分け）。

5. **career-portfolio を横断ハブへ移行**（本セッション後半・上記「現在地」）
   - development/ へ移動、**会社単位ディレクトリ**（`appeals/<会社>/`・`sources/<会社>/`）へ再配置、ドキュメントを SUPERNOVA 非依存に一般化。
   - **入力ソースを柔軟化**（`CLAUDE.md`「入力ソースの取り方（柔軟）」）。理由: tasks 資料は SUPERNOVA 固有の運用で、別プロジェクトには無い。基本＝ファイルメンション、無ければ **git 履歴から棚卸し／本人メモ／将来 Slack／その場でメモ作成**。

---

## 4. 確立したルール・仕組み（新セッションが従う要点）

- **2つの出力形式**: ①進行中ドラフト（`appeal-template.md`）→ ②完了・転記時にレジュメ完成版（`resume-template.md`）へ変換＋抽象化。
- **固有名**: 進行中は実名／完了・レジュメ化で抽象化（一般技術名 Go/Slack 等は抽象化しない）。
- **取捨選択**: 候補を5軸でストック → 応募先で上位2〜3点を理由付き選定（`selection-request-template.md`）。
- **入力**: 基本ファイルメンション、無ければ git 履歴等で柔軟に素材化。事実の裏付けは入力から、無い数値はプレースホルダー。
- **保存先**: `appeals/<会社>/<YYYY-MM-テーマ>.md`。
- **CLAUDE.md は自動ロードされない**（career-portfolio 外で作業するため）。毎回 `@career-portfolio/CLAUDE.md` を明示メンション。

---

## 5. これからやること

### A. 未コミット差分の確定（最優先）
§0 の通り。本人の指示でコミット → 確認の上 push。

### B. 別プロジェクトを `development/` へ移動（本人の次タスク）
career-portfolio の移動と同じ要領で安全に行う。career-portfolio 側は**変更不要**（別プロジェクトが兄弟に並べば、そのままメンションして整理を依頼できる）。

移動手順の指針（career-portfolio 移動時に実証済み）:
1. 事前チェック: 対象が git リポジトリか・`git status` がクリーンか・リモート同期済みか／移動先 `~/Applications/development/<name>` が空か。
2. 移動元の**外**にいる状態で `mv <現パス> ~/Applications/development/<name>`。
3. 移動後: `git rev-parse --show-toplevel` / `git remote -v` / `git status -sb` で健全性確認。
4. 親が git 管理外（submodule でない）なら影響なし。submodule の場合は別途配慮。

> 移動が済んだら、そのプロジェクトのレジュメ経験は `START-HERE.md` のキックオフ（資料が無ければ「git 履歴から棚卸し版」）で着手できる。保存先は `appeals/<その会社>/`。

### C. 既存ドラフトの仕上げ（随時）
- `appeals/supernova/` の3件はいずれも**下書き（数値・固有名にプレースホルダー）**。実データを埋めて完成へ。
- **delete-order-batch はリリース後**に、成果の定量（再発防止の実証・運用工数削減・承認→PRのリードタイム）と候補表 #1/#4 の「暫定」スコアを更新。

---

## 6. ファイル地図

| ファイル | 役割 |
|---|---|
| `CLAUDE.md` | AI 方針・入力チェックリスト・出力フォーマット(A/B)・固有名ライフサイクル・取捨選択ルーブリック(5軸) |
| `README.md` | 目的・配置・構成・運用フロー |
| `START-HERE.md` | セッション開始ルール（横断ルートで開く・キックオフ文2種） |
| `handover.md` | この引き継ぎ資料 |
| `templates/appeal-template.md` | 進行中ドラフト用（候補表セクション込み） |
| `templates/resume-template.md` | レジュメ完成版（課題/解決策/成果/関与度/使用技術 × 詳細/圧縮） |
| `templates/selection-request-template.md` | 応募先に合わせた2〜3点の選択依頼フォーム |
| `templates/request-template.md` | 整理依頼フォーム |
| `appeals/supernova/2026-06-*.md` | 既存3件（ai-goal-management / order-csv-refactor / delete-order-batch） |

---

## 7. 注意点

- **git 操作は本人指示後のみ・push 前に確認**（このリポジトリ＝個人 private、横断で各社の固有名を含むため特に慎重に）。
- **会社をまたぐ抽象化**: 各社の社名・パートナー名・人物名・社内固有名はレジュメ転記時に抽象化（用語対応表を使う）。
- **セッションは `~/Applications/development/` で開く**（supernova-biz 配下からは `@career-portfolio/...` が届かない）。
