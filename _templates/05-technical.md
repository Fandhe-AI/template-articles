# テクニカル基盤・構造化データ: <記事名>

## JSON-LD スキーママークアップ

### Article スキーマ
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "",
  "description": "",
  "author": {
    "@type": "Person",
    "name": ""
  },
  "publisher": {
    "@type": "Organization",
    "name": ""
  },
  "datePublished": "",
  "dateModified": "",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": ""
  }
}
```

### FAQPage スキーマ
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": ""
      }
    }
  ]
}
```

### その他のスキーマ
<!-- 記事内容に応じて HowTo, BreadcrumbList 等を追加 -->

## セマンティック HTML マークアップ指示

### 推奨 HTML 構造
```html
<article>
  <header>
    <h1><!-- タイトル --></h1>
    <meta content="<!-- 公開日 -->">
  </header>
  <section>
    <h2><!-- 質問ベース見出し --></h2>
    <!-- モジュール化されたコンテンツ -->
  </section>
  <!-- セクションを繰り返し -->
  <section itemscope itemtype="https://schema.org/FAQPage">
    <!-- FAQ セクション -->
  </section>
</article>
```

### セマンティック要素の使用指示
| 要素 | 用途 | 適用箇所 |
|------|------|---------|
| `<article>` | 記事本体のラッパー | ページ全体 |
| `<section>` | 各 H2 セクション | 全 H2 |
| `<nav>` | 目次 | ページ上部 |
| `<figure>` / `<figcaption>` | 画像・図表 | メディア要素 |
| `<blockquote cite="">` | 外部引用 | 引用箇所 |
| `<time datetime="">` | 日付 | 公開日・更新日 |

## 画像最適化指示

| 画像 | ファイル名 | Alt text | サイズ |
|------|----------|----------|--------|
| アイキャッチ | | | 1200x630px |
| | | | |

## 公開チェックリスト

### テクニカル SEO
- [ ] JSON-LD が Google Rich Results Test で有効
- [ ] canonical URL が正しく設定されている
- [ ] Open Graph タグ（og:title, og:description, og:image）が設定されている
- [ ] Twitter Card タグが設定されている

### AI クローラーアクセス
- [ ] robots.txt で PerplexityBot のアクセスが許可されている
- [ ] robots.txt で ChatGPT-User のアクセスが許可されている
- [ ] サイトマップに記事 URL が含まれている

### パフォーマンス
- [ ] Core Web Vitals を満たしている（LCP < 2.5s, INP < 200ms, CLS < 0.1）
- [ ] 画像が WebP/AVIF フォーマットで最適化されている
- [ ] モバイルフレンドリーである

### コンテンツ最終確認
- [ ] `lastModified` の日付が最新に設定されている
- [ ] 全外部リンクが有効（404 なし）
- [ ] 全内部リンクが有効

## 継続監視計画

### 定期更新スケジュール
- **四半期レビュー**: 最新データ・事例の追加、FAQ の更新
- **年次レビュー**: 記事全体の構造見直し

### AI 検索可視性トラッキング
<!-- 公開後に記録 -->
| 計測日 | プラットフォーム | クエリ | 引用有無 | メモ |
|--------|--------------|-------|---------|------|
| | ChatGPT | | | |
| | Perplexity | | | |
| | Google AI Overview | | | |

## 判定: 公開判定
- **結果**: 公開可 / 要修正 / 保留
- **公開予定日**: 
- **理由**: 
- **日付**: YYYY-MM-DD
