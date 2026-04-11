# Phase 2: AI 親和性アウトライン設計プロンプト

このプロンプトは **Strategist & Researcher** エージェントが Phase 2 で使用する。

## プロンプト構造

```xml
<context>
  <project_goal>
    AI 検索エンジンが情報を抽出しやすく、人間の読者にとっても消化しやすい
    アウトラインを設計する。各セクションが独立した回答として成立する
    モジュール構造を持たせる。
  </project_goal>

  <research_output>
    {{PHASE_1_RESEARCH_RESULT}}
  </research_output>

  <entity_list>
    {{ENTITY_LIST_WITH_PRIORITY}}
  </entity_list>

  <brand_guidelines>
    {{BRAND_IDENTITY_CONTENT}}
  </brand_guidelines>
</context>

<examples>
  <example>
    <description>質問ベース見出しとモジュール設計の例</description>
    <output>
      ## H2: オンラインカジノのライセンスとは何か？なぜ重要なのか？
      - 割り当てエンティティ: オンラインカジノ, ライセンス, 規制当局
      - セクション目的: 定義と重要性を結論ファーストで提示
      - 想定語数: 250 語
      - モジュール設計: 定義文（1文） → 重要性の根拠（数値） → 読者への示唆

      ## H2: 世界の主要なカジノライセンスにはどんな種類があるか？
      - 割り当てエンティティ: MGA, UKGC, Curacao, Gibraltar
      - セクション目的: 各ライセンスの比較を構造化データとして提示
      - 想定語数: 300 語
      - モジュール設計: 結論（最も信頼性が高いのは X） → 比較表 → 選択基準
    </output>
  </example>
</examples>

<instructions>
  以下のタスクを順に実行すること。**執筆は行わない**。構造設計のみ。

  1. **タイトル案の生成**
     - 主要エンティティを含み、CTR を最大化するタイトルを 3 案生成する
     - 各案に対して予想 CTR の根拠を簡潔に説明する

  2. **質問ベース見出しの設計**
     - H2 見出しは、読者が AI に問いかける自然な質問形式にする
     - H3 は H2 の質問を深掘りするサブ質問にする
     - 各 H2 に Phase 1 のエンティティを論理的に割り当てる

  3. **モジュール設計の指定**
     - 各セクションの論理構造を指定する（結論ファースト / 定義文 / ステップ解説 / 比較）
     - 各セクションの想定語数（200-300 語）を設定する
     - 含めるべき具体的なデータポイントを指定する

  4. **FAQ セクションの設計**
     - FAQPage スキーマに対応する Q&A を 3-5 件設計する
     - AI 検索で頻出する質問を優先する

  5. **内部リンク・トピッククラスター連携の計画**
     - ピラーページとの関係を定義する
     - 関連記事へのリンク先候補を提案する

  出力は `_templates/02-outline.md` のフォーマットに従うこと。
</instructions>
```

## 使い方

1. `{{PHASE_1_RESEARCH_RESULT}}` に Phase 1 の完成した `01-research.md` の内容を貼り付ける
2. `{{ENTITY_LIST_WITH_PRIORITY}}` にエンティティリスト（優先度付き）を貼り付ける
3. `{{BRAND_IDENTITY_CONTENT}}` に `.claude/rules/brand-identity.md` の内容を貼り付ける
4. Strategist & Researcher エージェントにプロンプト全体を渡す
5. **出力されたアウトラインは必ず人間がレビュー・修正する**（最重要ゲート）
