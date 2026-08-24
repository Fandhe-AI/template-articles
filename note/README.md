# note/

[note](https://note.com) 公開用の日本語記事下書きを管理するディレクトリ。本リポジトリでは**日本語の新規記事の公開先は note を既定とする**。

note には GitHub 連携がないため、ここは**公開可能な最終下書きの置き場**であり、公開は人間が本文を note エディタに貼り付けることで行う（見出し・引用・コードブロックは変換されるが、リンクの変換は不安定で、画像は反映されない。手順は `.claude/rules/platforms/note.md` の「公開モデル」）。**ローカルファイルが正、note 上は写し**として扱う（公開後の修正もローカルを先に直す）。

```text
note/
├── articles/
│   └── <slug>.md          日本語記事（yyyy-mm-<topic-kebab-case>.md、status: draft）
└── images/
    └── <yyyy-mm>/         GPT で生成した画像（人間が生成・保存する）
```

- 変換は `/note-publish <article-name>` で行う（`note-publisher` エージェント）
- 仕様・記法・画像サイズは `.claude/rules/platforms/note.md` を参照
- 各記事の frontmatter は本リポジトリの管理用。**note への貼り付け時に除去する**
- textlint は Zenn の設定を共用する: `cd zenn && pnpm exec textlint ../note/articles/<slug>.md`

> note の規約と AI の扱い（正典: `.claude/rules/platforms/note.md` の「最重要の制約」）に留意し、公開前に必ず人間が公式の規約・ヘルプを再確認のうえ全文を読み、自分の言葉として加筆修正すること。エージェントは公開操作・有料設定・AI 学習提供の設定を行わない。
