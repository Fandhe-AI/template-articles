# はじめ方

## 前提条件

- [Claude Code](https://claude.com/claude-code) がインストールされていること
- 本リポジトリがローカルにクローンされていること

## 初回セットアップ

### 1. ブランド情報の設定

記事作成を始める前に、以下の 2 ファイルを自社の情報で埋める。

#### `.claude/rules/brand-identity.md`

ブランドの基本情報とトーン＆マナーを定義する。

- ブランド名（正式名称）
- ミッション
- ターゲットオーディエンス
- 文体の原則（専門性、簡潔さ、具体性、中立性）
- 使用する表現 / 避けるべき表現

#### `.claude/rules/entity-dictionary.md`

記事で使用する固有名詞の正式名称辞書を作成する。

- 自社エンティティ（組織名、製品名、サービス名、人名）
- パートナー・関連企業
- 業界用語・専門用語

この辞書が AI エンジンによるエンティティ認識の精度を左右する。

### 2. サンプル記事の配置（任意）

過去のトップパフォーミング記事がある場合、`docs/samples/` に配置する。Module Creator エージェントが文体の参考として使用できる。

## 最初の記事を作成する

```bash
# Claude Code を起動
claude

# 新しい記事を作成（kebab-case で命名）
/new-article my-first-geo-article
```

このコマンドを実行すると:

1. `articles/my-first-geo-article/` ディレクトリが作成される
2. `README.md`（ステータストラッカー）が生成される
3. Phase 1（リサーチ）が自動的に開始される
4. Strategist & Researcher エージェントがターゲットキーワードを聞いてくる

## 基本的なワークフロー

```
/new-article <name>        → Phase 1 開始
（リサーチ作業）
/advance-phase <name>      → Phase 1 → 2 へ
（アウトライン作業）
/advance-phase <name>      → Phase 2 → 3 へ（人間の承認が必要）
（執筆作業）
/advance-phase <name>      → Phase 3 → 4 へ
（レビュー作業）
/advance-phase <name>      → Phase 4 → 5 へ
（テクニカル作業）
/advance-phase <name>      → Phase 5 完了 → 公開準備完了
```

## 進捗確認

```bash
# 特定記事のステータス
/article-status my-first-geo-article

# 全記事の一覧
/article-status

# 記事の全体サマリー
/article-summary my-first-geo-article
```

## 次のステップ

- [ワークフロー全体像](workflow-overview.md) — 5 フェーズと判定ゲートの詳細
- [各フェーズの詳細ガイド](phase-guide.md) — フェーズごとの進め方
- [GEO/SEO 基本原則](geo-seo-principles.md) — なぜこのワークフローが必要か
- [ベストプラクティス](best-practices.md) — プロンプト設計とトークン最適化
