---
name: article-summary
description: 記事の全フェーズを横断したサマリーを生成する
argument-hint: "<article-name> (例: geo-seo-optimization-guide)"
user-invocable: true
---

記事 `$ARGUMENTS` の全フェーズを横断したサマリーを生成する。

## 手順

1. `articles/$ARGUMENTS/` ディレクトリの存在を確認する

2. 以下のファイルを読み込む（存在するもののみ）:
   - `README.md` — ステータス
   - `01-research.md` — リサーチ結果
   - `02-outline.md` — アウトライン
   - `03-draft.md` — 草稿
   - `04-review.md` — レビュー結果
   - `05-technical.md` — テクニカル情報

3. 以下のフォーマットでサマリーを生成する:

   ```markdown
   # サマリー: <記事名>

   ## 基本情報
   - タイトル:
   - ターゲットキーワード:
   - 現在のフェーズ:
   - 最終更新日:

   ## Phase 1: リサーチ
   - AI 検索意図: （上位 3 件を要約）
   - 主要エンティティ: （高優先度のものを列挙）
   - 差別化ポイント: （3 件を要約）

   ## Phase 2: アウトライン
   - 採用タイトル:
   - H2 構造: （見出し一覧）
   - FAQ 件数:

   ## Phase 3: 執筆
   - 総語数:
   - セクション数:
   - 引用ソース数:

   ## Phase 4: レビュー
   - 修正指示数: （高/中/低の内訳）
   - 修正反映状況:

   ## Phase 5: テクニカル
   - 生成スキーマ: （Article, FAQPage 等）
   - 公開チェックリスト達成率:

   ## 次のアクション
   - （現フェーズに基づく推奨アクション）
   ```

4. ユーザーにサマリーを表示する
