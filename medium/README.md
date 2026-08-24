# medium/

[Medium](https://medium.com) 公開用の英語記事下書きを管理するディレクトリ。

Medium には GitHub 連携がないため、ここは**公開可能な最終下書きの置き場**であり、公開は人間が Markdown をリッチテキスト化して Medium エディタに貼り付けることで行う（raw Markdown の貼り付けでは記法が変換されない。手順は `.claude/rules/platforms/medium.md` の「公開モデル」）。**ローカルファイルが正、Medium 上は写し**として扱う（公開後の修正もローカルを先に直す）。

```text
medium/
├── articles/
│   └── <slug>.md          英語記事（yyyy-mm-<topic-kebab-case>.md、status: draft）
└── images/
    └── <yyyy-mm>/         GPT で生成した画像（人間が生成・保存する）
```

- 変換は `/medium-publish <article-name>` で行う（`medium-publisher` エージェント）
- 仕様・記法・GPT 画像プロンプトのテンプレートは `.claude/rules/platforms/medium.md` を参照
- 各記事の frontmatter は本リポジトリの管理用。**Medium への貼り付け時に除去する**

> Medium の AI コンテンツポリシー（正典: `.claude/rules/platforms/medium.md` の「最重要の制約」）に留意し、公開前に必ず人間が公式ポリシーを再確認のうえ全文を読み、自分の言葉として加筆修正すること。エージェントは公開操作を行わない。
