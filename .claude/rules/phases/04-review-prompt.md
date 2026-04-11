# Phase 4: セマンティック・レビュープロンプト

このプロンプトは **Auditor** エージェントが Phase 4 で使用する。

## プロンプト構造

```xml
<context>
  <project_goal>
    草稿のセマンティック品質を監査し、AI エンジンがコンテンツの文脈を正確に把握して
    引用できる状態に仕上げる。エンティティの一貫性、パワーフレーズの配置、
    内部リンク戦略を検証する。
  </project_goal>

  <draft>
    {{PHASE_3_DRAFT_CONTENT}}
  </draft>

  <entity_dictionary>
    {{ENTITY_DICTIONARY_CONTENT}}
  </entity_dictionary>

  <brand_guidelines>
    {{BRAND_IDENTITY_CONTENT}}
  </brand_guidelines>

  <research_data>
    {{PHASE_1_ENTITY_LIST}}
  </research_data>
</context>

<examples>
  <example>
    <description>エンティティ一貫性の問題検出</description>
    <output>
      問題: セクション 2 で「UKGC」、セクション 4 で「UK Gambling Commission」と
      表記が不統一。entity-dictionary では初出時にフル表記、以降は略称を許可。
      修正指示: セクション 4 の「UK Gambling Commission」を「UKGC」に統一する。
    </output>
  </example>
  <example>
    <description>パワーフレーズの追加推奨</description>
    <output>
      セクション 3 と 4 の間にトピック間の関係性シグナルが不足。
      修正指示: セクション 4 冒頭に「この規制強化の直接的な結果として」等の
      パワーフレーズを追加し、因果関係を明示する。
    </output>
  </example>
</examples>

<instructions>
  以下の監査タスクを順に実行すること。**文章の書き直しは行わない。修正指示のみ出力する。**

  1. **エンティティ一貫性チェック**
     - 記事内の全固有名詞を entity_dictionary と照合する
     - 表記揺れ・不整合を検出し、修正指示を出す
     - 初出時のフル表記 + 略称ルールが守られているか確認する

  2. **パワーフレーズ・セマンティックキューの監査**
     - 情報間の関係性を示すシグナル（因果、比較、時系列）が適切に配置されているか確認する
     - 不足箇所に対して具体的なパワーフレーズの追加を提案する
     - 過剰な接続詞や冗長な遷移表現を検出する

  3. **ブランドガイドライン準拠チェック**
     - brand_guidelines に定義されたトーン＆マナーとの整合性を検証する
     - 避けるべき表現が使用されていないか確認する

  4. **GEO/SEO 品質チェック**
     - 各セクションが単体で AI の回答として成立するか検証する
     - 結論ファーストの構造が徹底されているか確認する
     - 具体的なデータポイント（数値・固有名詞）の密度を評価する

  5. **内部リンク・トピッククラスター連携の提案**
     - 説明的なアンカーテキストによる内部リンクの配置を提案する
     - ピラーページへのリンクが含まれているか確認する

  出力は `_templates/04-review.md` のフォーマットに従い、修正指示リストを作成すること。
  各修正指示には優先度（高 / 中 / 低）を付与すること。
</instructions>
```

## 使い方

1. `{{PHASE_3_DRAFT_CONTENT}}` に Phase 3 の `03-draft.md` の本文を貼り付ける
2. `{{ENTITY_DICTIONARY_CONTENT}}` に `.claude/rules/entity-dictionary.md` の内容を貼り付ける
3. `{{BRAND_IDENTITY_CONTENT}}` に `.claude/rules/brand-identity.md` の内容を貼り付ける
4. `{{PHASE_1_ENTITY_LIST}}` に Phase 1 のエンティティリストを貼り付ける
5. Auditor エージェントにプロンプト全体を渡す
6. 出力された修正指示をもとに草稿を修正する（修正は人間 or Module Creator が実施）
