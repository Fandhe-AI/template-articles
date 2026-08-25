# Articles Repository

GEO（Generative Engine Optimization）と SEO を統合した記事作成のための Claude Repository。

AI 検索（ChatGPT Search, Perplexity AI, Google AI Overviews）と従来型検索の双方でヒット率を最大化する記事を、5 フェーズのワークフローで体系的に作成する。

## ワークフロー概要

```
Phase 1: リサーチ & エンティティ分析
  ▼ [方向性判定]
Phase 2: AI 親和性アウトライン設計
  ▼ [アウトライン承認] ← 最重要ゲート（人間レビュー必須）
Phase 3: ハイブリッド執筆
  ▼ [草稿完成判定]
Phase 4: セマンティック・レビュー
  ▼ [品質判定]
Phase 5: テクニカル基盤・構造化データ
  ├─ 自社サイト等 → Technical Translator（JSON-LD, OGP）
  ├─ Zenn        → Zenn Publisher（frontmatter, Zenn 記法, textlint）
  ├─ Medium      → Medium Publisher（英語化, Medium 記法, GPT 画像生成）
  └─ note        → note Publisher（note 記法, GPT 画像生成, textlint）※日本語の新規記事の既定
  ▼ [公開判定]
```

Phase 1-4 は公開先を問わず共通。Phase 5 だけがプラットフォームごとに分岐する。

## クイックスタート

```bash
# 1. 新しい記事を作成
/new-article <article-name>

# 2. Phase 1（リサーチ）が自動開始される

# 3. フェーズを進める
/advance-phase <article-name>

# 4. 進捗確認
/article-status <article-name>
```

## ディレクトリ構造

```
articles/
├── CLAUDE.md                      # GEO/SEO グローバルルール
├── README.md                      # 本ファイル
│
├── _templates/                    # フェーズテンプレート
│   ├── 01-research.md
│   ├── 02-outline.md
│   ├── 03-writing.md
│   ├── 04-review.md
│   ├── 05-technical.md
│   ├── 05-zenn.md                 # Zenn 公開時の Phase 5
│   ├── 05-medium.md               # Medium 公開時の Phase 5
│   └── 05-note.md                 # note 公開時の Phase 5
│
├── docs/
│   ├── guide/                     # ガイドドキュメント
│   │   ├── getting-started.md
│   │   ├── workflow-overview.md
│   │   ├── phase-guide.md
│   │   ├── geo-seo-principles.md
│   │   └── best-practices.md
│   └── samples/                   # 記事サンプル
│
├── articles/                      # 記事ディレクトリ
│   └── <article-name>/
│       ├── README.md              # ステータストラッカー
│       ├── 01-research.md
│       ├── 02-outline.md
│       ├── 03-draft.md
│       ├── 04-review.md
│       └── 05-technical.md        # または 05-zenn.md / 05-medium.md / 05-note.md
│
├── zenn/                          # Zenn の GitHub 連携用（pnpm）
│   ├── articles/                  # <slug>.md
│   ├── images/<yyyy-mm>/
│   ├── package.json               # zenn-cli / textlint / markdownlint
│   ├── .textlintrc.json
│   └── .markdownlint-cli2.jsonc
│
├── medium/                        # Medium 公開用の英語記事下書き
│   ├── articles/                  # <slug>.md（status: draft）
│   └── images/<yyyy-mm>/          # GPT 生成画像とプロンプト（<slug>-*.png / *.prompt.txt）
│
├── note/                          # note 公開用の日本語記事下書き（日本語の新規記事の既定）
│   ├── articles/                  # <slug>.md（status: draft）
│   └── images/<yyyy-mm>/          # GPT 生成画像とプロンプト（<slug>-*.png / *.prompt.txt）
│
├── scripts/
│   └── gen-image.sh               # Codex CLI（image_gen）経由の画像生成ラッパー
│
└── .claude/
    ├── agents/                    # マルチエージェント（8 ロール）
    ├── rules/                     # AI 向けルール・リソース
    │   ├── brand-identity.md
    │   ├── entity-dictionary.md
    │   ├── phases/                # XML 構造化プロンプト
    │   ├── platforms/             # 公開先プラットフォーム仕様（zenn.md, medium.md, note.md）
    │   └── schemas/               # JSON-LD テンプレート
    └── skills/                    # スキルコマンド
```

## マルチエージェント体制

| エージェント | 役割 | 担当フェーズ |
|------------|------|------------|
| Orchestrator | 編集長・進行管理 | 全フェーズ横断 |
| Strategist & Researcher | 戦略立案・情報収集 | Phase 1-2 |
| Module Creator | 執筆・チャンキング | Phase 3 |
| Auditor | 監査・品質保証 | Phase 4 |
| Technical Translator | 構造化データ変換 | Phase 5（自社サイト等） |
| Zenn Publisher | Zenn 公開変換 | Phase 5（Zenn） |
| Medium Publisher | Medium 公開変換（英語化） | Phase 5（Medium） |
| note Publisher | note 公開変換 | Phase 5（note） |

## スキル一覧

| コマンド | 説明 |
|---------|------|
| `/new-article <name>` | 新しい記事ディレクトリを作成し Phase 1 を開始 |
| `/advance-phase <name>` | 現フェーズを完了し次へ進行 |
| `/article-status [name]` | 記事の進捗状況を表示 |
| `/article-summary <name>` | 全フェーズの要約を生成 |
| `/zenn-publish <name>` | レビュー済み記事を Zenn 公開形式に変換 |
| `/medium-publish <name>` | レビュー済み記事を Medium 公開形式（英語）に変換 |
| `/note-publish <name>` | レビュー済み記事を note 公開形式（日本語）に変換 |

## note で公開する場合（日本語の新規記事の既定）

Phase 4 まで完了したら `/note-publish <name>` を実行する。`note/articles/<slug>.md` に日本語記事が `status: draft` の状態で生成され、`articles/<name>/05-note.md` に GPT 画像生成プロンプト（見出し画像 + 差し込み）と公開チェックリストが記録される。

- Phase 1 で note 内競合分析（上位記事の構成・ハッシュタグ）を追加で行う
- note は表・Mermaid・脚注・インラインコードに非対応のため、変換時に箇条書き・画像化指示に置き換える
- 画像はエージェントが `scripts/gen-image.sh`（Codex CLI の `image_gen`。ChatGPT ログインで動作、API キー不要）で生成し `note/images/<yyyy-mm>/` に保存する（見出し画像は 1280×670px 比率、生成は `1280x672`）。採否は人間が判断し、生成できない場合は人間が ChatGPT で生成する
- textlint は Zenn の設定を共用する: `cd zenn && pnpm exec textlint ../note/articles/<slug>.md`

> **note の規約と AI の扱い**の正典は [`.claude/rules/platforms/note.md`](.claude/rules/platforms/note.md) の「最重要の制約」（公式リンク・最終確認日つき。制度内容はここに複製しない）。公開前に人間が公式ページを再確認する。
> エージェントの担当は下書きファイルの作成と画像のローカル生成まで。人間が全文を読んで自分の言葉として加筆修正し、note エディタへの貼り付け・画像のアップロード・ハッシュタグ設定・有料設定・公開を人間が行う（貼り付け手順は同ファイルの「公開モデル」）。

## Zenn で公開する場合

Phase 4 まで完了したら `/zenn-publish <name>` を実行する。`zenn/articles/<slug>.md` が `published: false` の状態で生成される。

`zenn/` は zenn-cli・textlint・markdownlint がセットアップ済み（pnpm）。clone 直後は依存関係のインストールのみ行う。

```bash
cd zenn
pnpm install

pnpm run preview   # ローカルプレビュー（http://localhost:8000）
pnpm run lint      # textlint + markdownlint
```

zenn コマンドは必ず `zenn/` 内で実行する（リポジトリルートで実行すると、ワークフロー用の `articles/` を記事と誤認する）。

> **Zenn は生成 AI に記事を生成させて量産する行為を禁止している。**
> 本ワークフローは執筆支援であり、文責は人間にある。全文を読んで自分の言葉として責任を持てると判断したときにのみ、人間が `published: true` に変更して公開する。エージェントは公開を実行しない。

仕様は [`.claude/rules/platforms/zenn.md`](.claude/rules/platforms/zenn.md) を参照。

## Medium で公開する場合

Phase 4 まで完了したら `/medium-publish <name>` を実行する。`medium/articles/<slug>.md` に**英語記事**が `status: draft` の状態で生成され、`articles/<name>/05-medium.md` に GPT 画像生成プロンプト（サムネイル + 差し込み）と公開チェックリストが記録される。

- 記事本文は英語、管理ドキュメントは日本語
- Phase 1 で Medium 内競合分析（上位記事の構成・タグ・Publication 候補）を追加で行う
- Medium は表・Mermaid・脚注に非対応のため、変換時に箇条書き・画像化指示に置き換える
- 画像はエージェントが `scripts/gen-image.sh`（Codex CLI の `image_gen`。ChatGPT ログインで動作、API キー不要）で生成し `medium/images/<yyyy-mm>/` に保存する。採否は人間が判断し、生成できない場合は人間が ChatGPT で生成する

> **Medium の AI コンテンツポリシー**の正典は [`.claude/rules/platforms/medium.md`](.claude/rules/platforms/medium.md) の「最重要の制約」（公式リンク・最終確認日つき。制度内容はここに複製しない）。公開前に人間が公式ページを再確認する。
> エージェントの担当は下書きファイルの作成と画像のローカル生成まで。人間が全文を読んで自分の言葉として加筆修正し、リッチテキスト化した本文の Medium エディタへの貼り付け・画像のアップロード・タグ設定・公開を人間が行う（貼り付け手順は同ファイルの「公開モデル」）。

仕様と GPT 画像プロンプトのテンプレートは [`.claude/rules/platforms/medium.md`](.claude/rules/platforms/medium.md) を参照。

## 詳細ドキュメント

- [はじめ方](docs/guide/getting-started.md)
- [ワークフロー全体像](docs/guide/workflow-overview.md)
- [各フェーズの詳細ガイド](docs/guide/phase-guide.md)
- [GEO/SEO 基本原則](docs/guide/geo-seo-principles.md)
- [ベストプラクティス](docs/guide/best-practices.md)
