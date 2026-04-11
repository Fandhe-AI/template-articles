# Phase 3: ハイブリッド執筆プロンプト

このプロンプトは **Module Creator** エージェントが Phase 3 で使用する。

## プロンプト構造

```xml
<context>
  <project_goal>
    承認済みアウトラインに基づき、ファクトチェック済みデータのみを使用して
    GEO/SEO 最適化された記事を執筆する。各セクションは AI が抜き出して
    回答に使えるモジュール構造とする。
  </project_goal>

  <approved_outline>
    {{PHASE_2_APPROVED_OUTLINE}}
  </approved_outline>

  <fact_check_data>
    <section heading="{{H2_HEADING_1}}">
      <facts>
        {{PERPLEXITY_RESPONSE_1}}
      </facts>
      <citations>
        {{CITATION_URLS_1}}
      </citations>
    </section>
    <section heading="{{H2_HEADING_2}}">
      <facts>
        {{PERPLEXITY_RESPONSE_2}}
      </facts>
      <citations>
        {{CITATION_URLS_2}}
      </citations>
    </section>
    <!-- 全 H2 セクション分 -->
  </fact_check_data>

  <entity_dictionary>
    {{ENTITY_DICTIONARY_CONTENT}}
  </entity_dictionary>
</context>

<examples>
  <example>
    <description>Write to be quoted フォーマットの例（良い例）</description>
    <output>
      UK Gambling Commission（UKGC）は、世界で最も厳格なオンラインカジノ規制機関である。
      2024年の年次報告によれば、UKGC のライセンスを持つオペレーターは前年比で
      不正アクセスを 35% 削減し、プレイヤー保護の苦情件数は 2,400 件から 1,560 件に
      減少した。この実績は、ライセンス選択がプレイヤーの安全性に直結することを示している。
    </output>
  </example>
  <example>
    <description>避けるべきフォーマット（悪い例）</description>
    <output>
      オンラインカジノのライセンスについて知ることは非常に重要です。
      世界にはたくさんのライセンスがあり、それぞれに特徴があります。
      良いライセンスを持つカジノを選ぶことで、安全にプレイできるでしょう。
    </output>
  </example>
</examples>

<instructions>
  以下のルールに厳格に従って各セクションを執筆すること。

  **絶対ルール:**
  - 提供された fact_check_data のみを使用して文章を構成する
  - 事実データにない情報を追加・捏造しない
  - 各セクションは 200-300 語のモジュールとして完結させる

  **文体ルール:**
  - 結論ファースト: 物語的な導入を避け、直接的な主張から始める
  - Write to be quoted: 主張 → 名前付きデータポイント → 明確な示唆
  - 曖昧な形容詞（「優れた」「素晴らしい」「最高の」）を使用しない
  - 不自然なキーワードの繰り返しを避け、多様な自然表現を使用する
  - entity_dictionary の正式名称を一貫して使用する

  **構造ルール:**
  - 権威ある外部ソースからの引用を各セクション 1-3 件、本文中に自然に組み込む
  - 引用は [ソース名](URL) の形式でインラインリンクとする
  - FAQ セクションは簡潔かつ直接的に回答する（1 回答 50-100 語）

  出力は `_templates/03-writing.md` のフォーマットに従うこと。
</instructions>
```

## 使い方

1. `{{PHASE_2_APPROVED_OUTLINE}}` に人間が承認した `02-outline.md` の内容を貼り付ける
2. 各 H2 セクションのトピックについて Perplexity AI 等でファクトチェック済み情報を取得する
3. `{{PERPLEXITY_RESPONSE_N}}` と `{{CITATION_URLS_N}}` に取得したデータを貼り付ける
4. `{{ENTITY_DICTIONARY_CONTENT}}` に `.claude/rules/entity-dictionary.md` の内容を貼り付ける
5. Module Creator エージェントにプロンプト全体を渡す
