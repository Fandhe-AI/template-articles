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
  ▼ [公開判定]
```

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
│   └── 05-technical.md
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
│       └── 05-technical.md
│
└── .claude/
    ├── agents/                    # マルチエージェント（5 ロール）
    ├── rules/                     # AI 向けルール・リソース
    │   ├── brand-identity.md
    │   ├── entity-dictionary.md
    │   ├── phases/                # XML 構造化プロンプト
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
| Technical Translator | 構造化データ変換 | Phase 5 |

## スキル一覧

| コマンド | 説明 |
|---------|------|
| `/new-article <name>` | 新しい記事ディレクトリを作成し Phase 1 を開始 |
| `/advance-phase <name>` | 現フェーズを完了し次へ進行 |
| `/article-status [name]` | 記事の進捗状況を表示 |
| `/article-summary <name>` | 全フェーズの要約を生成 |

## 詳細ドキュメント

- [はじめ方](docs/guide/getting-started.md)
- [ワークフロー全体像](docs/guide/workflow-overview.md)
- [各フェーズの詳細ガイド](docs/guide/phase-guide.md)
- [GEO/SEO 基本原則](docs/guide/geo-seo-principles.md)
- [ベストプラクティス](docs/guide/best-practices.md)
