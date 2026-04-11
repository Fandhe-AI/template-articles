# Phase 5: テクニカル基盤・構造化データプロンプト

このプロンプトは **Technical Translator** エージェントが Phase 5 で使用する。

## プロンプト構造

```xml
<context>
  <project_goal>
    完成した記事を機械（AI エンジン）が直接読み取れる技術フォーマットに変換する。
    JSON-LD スキーママークアップの自動生成、セマンティック HTML の指示、
    公開チェックリストの作成を行う。
  </project_goal>

  <final_article>
    {{PHASE_4_REVIEWED_ARTICLE}}
  </final_article>

  <article_metadata>
    <title>{{ARTICLE_TITLE}}</title>
    <description>{{META_DESCRIPTION}}</description>
    <author>{{AUTHOR_NAME}}</author>
    <publish_date>{{PUBLISH_DATE}}</publish_date>
    <canonical_url>{{CANONICAL_URL}}</canonical_url>
    <language>{{LANGUAGE_CODE}}</language>
  </article_metadata>

  <schema_templates>
    <template type="Article">
      {{ARTICLE_SCHEMA_TEMPLATE}}
    </template>
    <template type="FAQPage">
      {{FAQ_SCHEMA_TEMPLATE}}
    </template>
    <template type="BreadcrumbList">
      {{BREADCRUMB_SCHEMA_TEMPLATE}}
    </template>
  </schema_templates>
</context>

<examples>
  <example>
    <description>Article + FAQPage 統合スキーマの出力例</description>
    <output>
      [
        {
          "@context": "https://schema.org",
          "@type": "Article",
          "headline": "オンラインカジノライセンスの種類と選び方ガイド",
          "author": {"@type": "Person", "name": "田中太郎"},
          "datePublished": "2026-04-11",
          "dateModified": "2026-04-11"
        },
        {
          "@context": "https://schema.org",
          "@type": "FAQPage",
          "mainEntity": [
            {
              "@type": "Question",
              "name": "最も信頼性の高いオンラインカジノライセンスはどれですか？",
              "acceptedAnswer": {
                "@type": "Answer",
                "text": "UKGC（UK Gambling Commission）が最も厳格な規制を持ち..."
              }
            }
          ]
        }
      ]
    </output>
  </example>
</examples>

<instructions>
  以下のタスクを順に実行すること。**コンテンツの編集は行わない。技術変換のみ。**

  1. **JSON-LD スキーママークアップの生成**
     - 記事本文と metadata を解析する
     - schema_templates を使用して以下のスキーマを生成する:
       - Article スキーマ（必須）
       - FAQPage スキーマ（FAQ セクションがある場合）
       - HowTo スキーマ（手順解説がある場合）
       - BreadcrumbList スキーマ（必須）
     - プレースホルダー `{{...}}` を実際の値で置換する

  2. **セマンティック HTML マークアップ指示**
     - 記事構造に対応するセマンティック HTML 要素を指定する
     - `<article>`, `<section>`, `<nav>`, `<figure>`, `<time>` 等の使用箇所を明示する

  3. **画像最適化指示**
     - アイキャッチ画像の仕様を出力する（1200x630px, WebP/AVIF）
     - 各画像に説明的なファイル名と Alt text を提案する

  4. **公開チェックリストの作成**
     - テクニカル SEO（canonical, OGP, Twitter Card）
     - AI クローラーアクセス（robots.txt, sitemap）
     - パフォーマンス（Core Web Vitals）
     - コンテンツ最終確認（リンク有効性, lastModified）

  5. **継続監視計画の策定**
     - 四半期レビューのスケジュール
     - AI 検索可視性トラッキングの初期設定

  出力は `_templates/05-technical.md` のフォーマットに従うこと。
</instructions>
```

## 使い方

1. `{{PHASE_4_REVIEWED_ARTICLE}}` にレビュー済み最終版記事を貼り付ける
2. `{{ARTICLE_TITLE}}` 等のメタデータを入力する
3. `{{ARTICLE_SCHEMA_TEMPLATE}}` 等に `.claude/rules/schemas/` のテンプレートを貼り付ける
4. Technical Translator エージェントにプロンプト全体を渡す
