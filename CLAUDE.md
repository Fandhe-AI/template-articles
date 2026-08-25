# Articles Repository

GEO（Generative Engine Optimization）と SEO を統合し、AI 検索と従来型検索の双方でヒット率を最大化する記事を体系的に作成するリポジトリ。

## ワークフロー

記事は以下の 5 フェーズを順に進行する。フェーズ間には判定ゲートがある。

| フェーズ | ファイル | 担当エージェント | 内容 |
|---------|---------|----------------|------|
| 1. リサーチ | `01-research.md` | Strategist & Researcher | 競合分析、エンティティ抽出、AI 検索意図マッピング |
| 2. アウトライン | `02-outline.md` | Strategist & Researcher | 質問ベース見出し、モジュール設計、CTR 最適化タイトル |
| 3. 執筆 | `03-draft.md` | Module Creator | ファクトデータに基づく構造化執筆 |
| 4. レビュー | `04-review.md` | Auditor | エンティティ一貫性監査、パワーフレーズ、内部リンク |
| 5. テクニカル | `05-technical.md` | Technical Translator | JSON-LD 生成、マークアップ指示、公開チェックリスト |

### Phase 5 のプラットフォーム分岐

Phase 5 は**公開先プラットフォームによって担当エージェントが変わる**。Phase 1-4 は共通。

| 公開先 | ファイル | 担当エージェント | 内容 |
| --- | --- | --- | --- |
| 自社サイト等（デフォルト） | `05-technical.md` | Technical Translator | JSON-LD、セマンティック HTML、OGP |
| Zenn | `05-zenn.md` | Zenn Publisher | frontmatter、Zenn 独自記法、textlint、公開チェックリスト |
| Medium | `05-medium.md` | Medium Publisher | 英語化、Medium 互換記法、title/tags 設計、GPT 画像生成（プロンプト + `scripts/gen-image.sh`）、公開チェックリスト |
| note | `05-note.md` | note Publisher | note 互換記法、title/hashtags 設計、GPT 画像生成（プロンプト + `scripts/gen-image.sh`）、textlint、公開チェックリスト |

**日本語の新規記事の公開先は note を既定とする**（Zenn の仕組みは Zenn へ出す記事のために残す）。

Zenn・Medium・note は JSON-LD・OGP・sitemap を自動生成・管理するため、これらの手動生成は行わない。canonical は Zenn では不要（記事 URL が canonical）。Medium では初出なら Medium の記事 URL がそのまま canonical になるが、**クロスポスト時は人間が Import ツールまたは Story settings で元記事の URL を設定する**。note には canonical の設定機能がないため、**クロスポストは重複コンテンツになる。note へは原則初出の記事を出す**。仕様は `.claude/rules/platforms/zenn.md` / `.claude/rules/platforms/medium.md` / `.claude/rules/platforms/note.md` を参照。

Zenn の作業ディレクトリは `zenn/`（パッケージマネージャは **pnpm**、zenn-cli / textlint / markdownlint がセットアップ済み）。**zenn コマンドは必ず `cd zenn` してから実行する**（ルートで実行するとワークフロー用の `articles/` を記事と誤認する）。`zenn init` は再実行しない。

**Zenn は生成 AI による記事の量産を禁止している。** `published: true` への変更と Zenn 連携ブランチへの push は、人間が全文を確認したうえで人間が行う。エージェントは実行しない。Phase 5 は `published: false` のままで完了する。

**note は日本語記事の既定の公開先。** note には GitHub 連携がなく、公開は人間が本文を note エディタに貼り付けることで行う（見出し・引用・コードブロックは変換されるが、リンクの変換は不安定で画像は反映されない。手順は `.claude/rules/platforms/note.md` の「公開モデル」）。note の規約と AI の扱い（正典: `.claude/rules/platforms/note.md` の「最重要の制約」。公開前に人間が公式ページを再確認する）を踏まえ、エージェントは `note/articles/<slug>.md`（`status: draft`・`paid: false`）の作成と、画像の生成・ローカル保存（`note/images/<yyyy-mm>/`）までを担当し、生成画像の採否・貼り付け・画像アップロード・有料設定・AI 学習提供の設定・公開は人間が行う。出力先が note の場合、Phase 1 で note 内競合分析（上位記事・ハッシュタグ）を追加で行う。textlint は `zenn/.textlintrc.json` を共用する（`cd zenn && pnpm exec textlint ../note/articles/<slug>.md`）。

**Medium は英語記事**（管理ドキュメントは日本語）。Medium には GitHub 連携がなく、公開は人間が Markdown をリッチテキスト化して Medium エディタに貼り付けることで行う（手順は `.claude/rules/platforms/medium.md` の「公開モデル」）。Medium の AI コンテンツポリシー（正典: `.claude/rules/platforms/medium.md` の「最重要の制約」。公開前に人間が公式ページを再確認する）を踏まえ、エージェントは `medium/articles/<slug>.md`（`status: draft`）の作成と、画像の生成・ローカル保存（`medium/images/<yyyy-mm>/`）までを担当し、生成画像の採否・貼り付け・画像アップロード・公開は人間が行う。出力先が Medium の場合、Phase 1 で Medium 内競合分析（上位記事・タグ・Publication）を追加で行う。

**画像生成は `scripts/gen-image.sh` で行う。** Codex CLI の組み込み `image_gen` ツールを `codex exec` で非対話実行するラッパーで、ChatGPT アカウントで `codex login` 済みなら動く（`OPENAI_API_KEY` は不要。ChatGPT プランの Codex 利用枠を消費する）。手順・確認観点・失敗時のフォールバックの正典は `.claude/rules/platforms/medium.md` の「画像（GPT 生成）」（note も共用）。**数値・文字を含む表やグラフには使わない**（作図・スクリーンショット）。生成に失敗した場合、エージェントは API キーの設定や代替ツールの導入を行わず「手動生成待ち」と記録して Phase 5 を続行する。画像ファイルの有無は Phase 5 の完了条件に含めない。

### 判定ゲート

- Phase 1→2: 方向性判定（差別化ポイントは明確か）
- Phase 2→3: アウトライン承認（**人間レビュー必須** — 最重要ゲート）
- Phase 3→4: 草稿完成判定（全セクション執筆済みか）
- Phase 4→5: 品質判定（GEO/SEO 基準を満たすか）
- Phase 5 完了: 公開判定

### フィードバックループ

フェーズは逆戻り可能。README.md のステータスで「🔁 再検討中」を使用する。

## マルチエージェント体制

エージェントは**役割（Role）**で分離される。各エージェントには明確な責任境界がある。

| エージェント | 役割 | やらないこと |
|------------|------|------------|
| `orchestrator` | 編集長 — 進行管理、コンテキスト引き継ぎ、判定ゲート | 個別タスクの実行 |
| `strategist-researcher` | 戦略・情報収集 — Phase 1-2 | 執筆 |
| `module-creator` | 執筆・チャンキング — Phase 3 | リサーチ、SEO 最適化 |
| `auditor` | 監査・品質保証 — Phase 4 | 文章の書き直し（修正指示のみ） |
| `technical-translator` | 構造化データ変換 — Phase 5（自社サイト等） | コンテンツ編集 |
| `zenn-publisher` | Zenn 公開変換 — Phase 5（Zenn） | コンテンツ編集、JSON-LD 生成、公開の実行 |
| `medium-publisher` | Medium 公開変換（英語化）・画像生成 — Phase 5（Medium） | コンテンツの意味を変える編集、JSON-LD 生成、画像のアップロード、公開の実行 |
| `note-publisher` | note 公開変換・画像生成 — Phase 5（note） | コンテンツの意味を変える編集、JSON-LD 生成、画像のアップロード、公開・有料設定の実行 |

## GEO 執筆ルール

- **結論ファースト**: 物語的な導入を避け、直接的な主張から始める
- **モジュール構造**: 各セクション 200-300 語、単体で AI の回答として成立する独立チャンク
- **Write to be quoted**: 主張 → 名前付きデータポイントで裏付け → 明確な示唆
- **エンティティ一貫性**: 固有名詞は `.claude/rules/entity-dictionary.md` の正式名称を使用
- **曖昧な形容詞を排除**: 「優れた」「最高の」ではなく具体的な数値・事実を使用
- **質問ベース見出し**: H2/H3 はユーザーが AI に問いかける自然な質問形式

## ディレクトリ構造

```
_templates/          フェーズテンプレート（01-05）
docs/guide/          ガイドドキュメント
docs/samples/        トップパフォーミング記事サンプル
articles/<name>/     各記事（kebab-case）
zenn/                Zenn の GitHub 連携用（articles/, images/）
medium/              Medium 公開用の英語記事下書き（articles/, images/）
note/                note 公開用の日本語記事下書き（articles/, images/）
scripts/             補助スクリプト（gen-image.sh: Codex CLI 経由の画像生成）
.claude/agents/      マルチエージェント定義
.claude/rules/       AI 向けルール・リソース
  ├── phases/        フェーズ別 XML 構造化プロンプト
  ├── platforms/     公開先プラットフォーム仕様（zenn.md, medium.md, note.md）
  ├── schemas/       JSON-LD テンプレート
  ├── brand-identity.md
  └── entity-dictionary.md
.claude/skills/      スキル定義
```

## スキル

- `/new-article <name>`: 新しい記事ディレクトリを作成し Phase 1 を開始
- `/advance-phase <article-name>`: 現フェーズを完了し次へ進行
- `/article-status [article-name]`: 記事の進捗状況を表示
- `/article-summary <article-name>`: 全フェーズの要約を生成
- `/zenn-publish <article-name>`: レビュー済み記事を Zenn 公開形式（`zenn/articles/<slug>.md`）に変換
- `/medium-publish <article-name>`: レビュー済み記事を Medium 公開形式（英語、`medium/articles/<slug>.md`）に変換
- `/note-publish <article-name>`: レビュー済み記事を note 公開形式（日本語、`note/articles/<slug>.md`）に変換

## 規約

- 記事名: kebab-case（例: `geo-seo-optimization-guide`）
- ドキュメント: 日本語で記述
- 各フェーズのドキュメントは `_templates/` のテンプレートに従う
- README.md の status フィールドを常に最新に保つ
- git commit は各フェーズの区切りで行う
