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
  └─ Medium      → Medium Publisher（英語化, Medium 記法, GPT 画像プロンプト）
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
│   └── 05-medium.md               # Medium 公開時の Phase 5
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
│       └── 05-technical.md        # または 05-medium.md
│
├── medium/                        # Medium 公開用の英語記事下書き
│   ├── articles/                  # <slug>.md（status: draft）
│   └── images/<yyyy-mm>/          # GPT 生成画像（人間が生成・保存）
│
└── .claude/
    ├── agents/                    # マルチエージェント（6 ロール）
    ├── rules/                     # AI 向けルール・リソース
    │   ├── brand-identity.md
    │   ├── entity-dictionary.md
    │   ├── phases/                # XML 構造化プロンプト
    │   ├── platforms/             # 公開先プラットフォーム仕様（medium.md）
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
| Medium Publisher | Medium 公開変換（英語化） | Phase 5（Medium） |

## スキル一覧

| コマンド | 説明 |
|---------|------|
| `/new-article <name>` | 新しい記事ディレクトリを作成し Phase 1 を開始 |
| `/advance-phase <name>` | 現フェーズを完了し次へ進行 |
| `/article-status [name]` | 記事の進捗状況を表示 |
| `/article-summary <name>` | 全フェーズの要約を生成 |
| `/medium-publish <name>` | レビュー済み記事を Medium 公開形式（英語）に変換 |

## Medium で公開する場合

Phase 4 まで完了したら `/medium-publish <name>` を実行する。`medium/articles/<slug>.md` に**英語記事**が `status: draft` の状態で生成され、`articles/<name>/05-medium.md` に GPT 画像生成プロンプト（サムネイル + 差し込み）と公開チェックリストが記録される。

- 記事本文は英語、管理ドキュメントは日本語
- Phase 1 で Medium 内競合分析（上位記事の構成・タグ・Publication 候補）を追加で行う
- Medium は表・Mermaid・脚注に非対応のため、変換時に箇条書き・画像化指示に置き換える
- 画像は人間が GPT で生成し `medium/images/<yyyy-mm>/` に保存する

> **Medium の AI コンテンツポリシー**の正典は [`.claude/rules/platforms/medium.md`](.claude/rules/platforms/medium.md) の「最重要の制約」（公式リンク・最終確認日つき。制度内容はここに複製しない）。公開前に人間が公式ページを再確認する。
> エージェントの担当は下書きファイルの作成まで。人間が全文を読んで自分の言葉として加筆修正し、リッチテキスト化した本文の Medium エディタへの貼り付け・タグ設定・公開を人間が行う（貼り付け手順は同ファイルの「公開モデル」）。

仕様と GPT 画像プロンプトのテンプレートは [`.claude/rules/platforms/medium.md`](.claude/rules/platforms/medium.md) を参照。

## 詳細ドキュメント

- [はじめ方](docs/guide/getting-started.md)
- [ワークフロー全体像](docs/guide/workflow-overview.md)
- [各フェーズの詳細ガイド](docs/guide/phase-guide.md)
- [GEO/SEO 基本原則](docs/guide/geo-seo-principles.md)
- [ベストプラクティス](docs/guide/best-practices.md)
