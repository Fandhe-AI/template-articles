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
   - `05-technical.md` — テクニカル情報（自社サイト等に公開する場合）
   - `05-zenn.md` — Zenn 公開変換（Zenn に公開する場合）
   - `05-medium.md` — Medium 公開変換（Medium に公開する場合）
   - `05-note.md` — note 公開変換（note に公開する場合）

   Phase 5 は公開先によってファイルが分岐する。**存在するものを読む**。

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
   <!-- 05-technical.md がある場合（自社サイト等） -->
   - 公開先: 自社サイト等
   - 生成スキーマ: （Article, FAQPage 等）
   - 公開チェックリスト達成率:

   <!-- 05-zenn.md がある場合（Zenn） -->
   - 公開先: Zenn
   - slug / 公開 URL:
   - frontmatter: （title, emoji, type, topics）
   - Lint 結果: （textlint / markdownlint）
   - 公開状態: （`published: false` = 未公開。公開は人間が判断する）

   <!-- 05-medium.md がある場合（Medium） -->
   - 公開先: Medium
   - slug（ファイル管理用）:
   - メタデータ: （title, subtitle, tags）
   - 画像プロンプト: （feature 1 件 + 差し込み N 件）
   - 公開状態: （`status: draft` = 未公開。画像生成・貼り付け・公開は人間が行う）

   <!-- 05-note.md がある場合（note） -->
   - 公開先: note
   - slug（ファイル管理用）:
   - メタデータ: （title, hashtags）
   - 画像プロンプト: （見出し画像 1 件 + 差し込み N 件）
   - textlint 結果:
   - 公開状態: （`status: draft` = 未公開。画像生成・貼り付け・公開は人間が行う）

   ## 次のアクション
   - （現フェーズに基づく推奨アクション）
   ```

4. ユーザーにサマリーを表示する
