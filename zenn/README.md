# zenn/

Zenn（<https://zenn.dev>）の GitHub 連携用ディレクトリ。`/zenn-publish <article-name>` の出力先。

## 構造

```text
zenn/
├── articles/                   記事本体（<slug>.md）
├── images/<yyyy-mm>/           画像
├── package.json                zenn-cli / textlint / markdownlint（pnpm）
├── .textlintrc.json            日本語校正ルール
└── .markdownlint-cli2.jsonc    Markdown 構文ルール
```

## セットアップ

初期化は完了している。**`zenn init` を再実行しないこと**（この README を上書きする）。clone 直後は依存関係のインストールのみ行う。

```bash
cd zenn
pnpm install
```

Zenn の GitHub 連携は**リポジトリ直下の `articles/` / `books/` を参照し、設定できるのは同期ブランチのみ**（サブディレクトリを指定する設定はない。正典: [GitHub リポジトリ連携ドキュメント](https://zenn.dev/zenn/articles/connect-to-github)）。本リポジトリはルート直下にワークフロー用の `articles/` があるため、`main` を同期ブランチにしてはならない。公開時は人間が次のいずれかを行う。

- **公開専用ブランチ方式（推奨）**: `zenn/` の内容をルートに持つ `publish` ブランチを次のコマンドで更新し、Zenn の同期ブランチに `publish` を設定する（subtree split はコミット SHA を出力するため、ローカルブランチを作らず公開のたびに繰り返し実行できる）

  ```bash
  # SHA を変数に取り、split が失敗・空のときは push しない
  # （空の refspec はリモートブランチの削除になる）
  SPLIT_SHA=$(git subtree split --prefix zenn HEAD) &&
  git push -f origin "${SPLIT_SHA:?subtree split failed}:refs/heads/publish"
  ```


- **別リポジトリ方式**: Zenn 連携専用のリポジトリを作り、公開時に `zenn/` の内容をそのルートへコピーして push する

## コマンド

**すべて `zenn/` 内で実行する。** リポジトリルートから実行すると、Zenn がルート直下のワークフロー用 `articles/`（`01-research.md` 等）を記事ディレクトリと誤認する。

```bash
cd zenn

pnpm run preview                # ローカルプレビュー（http://localhost:8000）
pnpm run lint                   # textlint + markdownlint
pnpm run lint:fix               # 自動修正できるものを修正
pnpm exec zenn new:article --slug 2026-07-example --title "タイトル" --type tech --emoji 🌊
```

`articles/` に記事が 1 件もない状態で `pnpm run lint` を実行すると、textlint が `SearchFilesNoTargetFileError`（対象ファイルなし）で終了する。記事を作成すれば解消する。

## 公開

記事は `published: false` の状態で出力される。**人間が全文を確認し、自分の言葉として責任を持てると判断したときにのみ** `published: true` に変更してコミット・push する。

> Zenn は生成 AI に記事を生成させて量産する行為を禁止している。本リポジトリのワークフローは執筆支援であり、文責は人間にある。

仕様の詳細は [`.claude/rules/platforms/zenn.md`](../.claude/rules/platforms/zenn.md) を参照。
