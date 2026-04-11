# Phase 1: リサーチ & エンティティ分析プロンプト

このプロンプトは **Strategist & Researcher** エージェントが Phase 1 で使用する。

## プロンプト構造

```xml
<context>
  <project_goal>
    GEO（Generative Engine Optimization）と SEO を統合した記事を作成するためのリサーチ。
    AI 検索エンジン（ChatGPT Search, Perplexity AI, Google AI Overviews）で引用される
    コンテンツの方向性を確立する。
  </project_goal>

  <documents>
    <document index="1">
      <source>競合記事 1</source>
      <document_content>
        {{COMPETITOR_ARTICLE_1}}
      </document_content>
    </document>
    <document index="2">
      <source>競合記事 2</source>
      <document_content>
        {{COMPETITOR_ARTICLE_2}}
      </document_content>
    </document>
    <document index="3">
      <source>競合記事 3</source>
      <document_content>
        {{COMPETITOR_ARTICLE_3}}
      </document_content>
    </document>
  </documents>

  <extracted_entities>
    {{NLP_ENTITY_LIST_FROM_SEO_TOOL}}
  </extracted_entities>

  <target_keyword>
    {{PRIMARY_KEYWORD}}
  </target_keyword>
</context>

<examples>
  <example>
    <input>キーワード: 「オンラインカジノ ライセンス」</input>
    <output>
      AI 検索意図:
      1. 「オンラインカジノのライセンスにはどんな種類がありますか？」
      2. 「最も信頼性の高いオンラインカジノライセンスはどれですか？」
      3. 「日本人がプレイできるライセンス付きカジノの見分け方は？」

      情報ギャップ:
      1. 競合は MGA と Curacao を言及するが、UKGC の最新規制変更（2024年）に触れていない
      2. ライセンスの取得コストの具体的な比較データが欠如
      3. AI 検索での引用実績がある権威あるソース（IAGR レポート等）への言及がない
    </output>
  </example>
</examples>

<instructions>
  以下のタスクを順に実行すること。**執筆は行わない**。分析と構造化のみ。

  1. **AI 検索意図のマッピング**
     - 提供された競合記事とキーワードを分析する
     - ユーザーが ChatGPT や Perplexity に入力しそうな自然言語の質問を 5 つ予測する
     - 最初の回答後にユーザーが続けて聞きそうなフォローアップ質問を 3 つ予測する

  2. **競合分析と情報ギャップの特定**
     - 競合記事の構造（見出し、エンティティ、データポイント）を分析する
     - 競合が言及していない情報ギャップを特定する
     - 独自の統計・調査で差別化できるポイントを 3 つ提案する

  3. **NLP エンティティの整理**
     - 提供されたエンティティリストを優先度別に分類する（高 / 中 / 低）
     - 記事内で使用すべきブランド固有エンティティを特定する

  4. **一次情報ソースの推奨**
     - 権威ある外部ソース（学術論文、公的機関レポート、業界調査）を提案する
     - AI 検索エンジンが引用しやすいソースを優先する

  出力は `_templates/01-research.md` のフォーマットに従うこと。
</instructions>
```

## 使い方

1. `{{COMPETITOR_ARTICLE_1-3}}` に上位競合記事のテキストデータを貼り付ける
2. `{{NLP_ENTITY_LIST_FROM_SEO_TOOL}}` に SEO ツールから抽出したエンティティリスト（CSV 等）を貼り付ける
3. `{{PRIMARY_KEYWORD}}` にターゲットキーワードを入力する
4. Strategist & Researcher エージェントにプロンプト全体を渡す
