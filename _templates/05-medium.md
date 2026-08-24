# Phase 5（Medium）: Medium 公開変換

- 記事: `<article-name>`
- 担当エージェント: medium-publisher
- 更新日: YYYY-MM-DD
- 出力先: `medium/articles/<slug>.md`（英語、`status: draft`）

## 1. slug（ファイル管理用。URL は Medium が自動生成）

| 項目 | 値 |
| --- | --- |
| slug | `yyyy-mm-topic-name` |
| クロスポスト | 初出 / クロスポスト（canonical: ） |

## 2. メタデータブロック

```yaml
---
title: ""
subtitle: ""
tags: []
canonical: ""
publication: ""
status: draft
---
```

### 設計根拠

| フィールド | 値 | 根拠 |
| --- | --- | --- |
| title | | 英語 3 案からユーザー選択。案と根拠: |
| subtitle | | 含めた数値・結論: |
| tags | | Phase 1 の Medium 内競合分析で確認した既存タグとの対応: |
| publication | | 候補と AI ポリシーの確認結果: |

## 3. Medium 互換記法への変換

| 箇所 | 変換前 | 変換後 | 理由 |
| --- | --- | --- | --- |
| セクション X | Markdown 表 | 箇条書き / 画像化指示 | Medium は表に非対応 |
| セクション Y | Mermaid | GPT イラスト / 作図指示 | Medium は Mermaid に非対応 |
| コードブロック | ```` ```ts:src/foo.ts ```` | ファイル名を地の文へ | Medium にファイル名記法なし |
| 脚注 | `[^1]` | インラインリンク / 括弧書き | Medium は脚注に非対応 |
| H4 以下 | | 太字段落 | Medium の見出しは 2 レベル |

### 英語化で表現を調整した箇所（意味は変えていないことの確認）

| 箇所 | 日本語原文の要旨 | 英語での表現 | 数値・留保の同一性 |
| --- | --- | --- | --- |
| | | | |

## 4. 画像（GPT 生成プロンプト）

生成・アップロードは人間が行う。生成後の確認観点: **文字化け / 矢印・関係の向き / 他社ロゴ混入**。

### 4.1 Feature image（サムネイル）

- 保存先: `medium/images/YYYY-MM/<slug>-feature.png`
- Alt text（英語）:
- キャプション案: （AI 生成の開示: "Image generated with AI" を含める）

```text
（プロンプト全文。末尾に medium.md のベーススタイルを含める）
```

### 4.2 差し込み画像

| # | 対象セクション | 種別（GPT 抽象図 / 作図 / スクショ） | 保存先 | Alt text |
| --- | --- | --- | --- | --- |
| 1 | | | | |

```text
（差し込み画像 1 のプロンプト全文）
```

### 4.3 画像化が必要な表・図（GPT 生成不可のもの）

| 箇所 | 内容 | 指示（作図ツール / スクリーンショット） |
| --- | --- | --- |
| | | |

## 5. 手動チェック結果

| 項目 | 結果 | 備考 |
| --- | --- | --- |
| 表 / Mermaid / 脚注 / H4 以下 / ネストリストの残存なし | | |
| コードブロックのファイル名記法なし | | |
| 単独行 URL なし（引用はインラインリンク） | | |
| リンク有効性 | | |
| エンティティ辞書との整合 | | |
| スペル・文法の通読確認 | | |

## 6. Phase 5 完了チェックリスト

**Phase 5 は「公開可能な英語下書き + 画像プロンプトを作る」までで完了する。** `status: draft` のまま完了してよい。公開そのものは Phase 5 の完了後に人間が行う（セクション 7 を参照）。

### 変換の妥当性

- [ ] title が 60 文字以内・タイトルケース・数値または結論を含む
- [ ] subtitle に主要な数値・結論がある
- [ ] tags が 5 件以内で、Medium の既存タグに合わせている
- [ ] 見出しが H2 / H3 のみ
- [ ] 冒頭 2 文で記事の価値を言い切っている
- [ ] TL;DR / Key Takeaways / References セクションがある
- [ ] `status` が `draft` である
- [ ] 事実・数値・留保が日本語版と一致している（変換で意味を変えていない）

### 画像

- [ ] Feature image のプロンプト・Alt text・キャプションがある
- [ ] 差し込み画像のプロンプトに共通ベーススタイルが付いている
- [ ] 数値を含む表・グラフを GPT 生成の対象にしていない

## 7. 公開手順（Phase 5 完了後、人間が実施）

**このセクションは Phase 5 の完了条件に含まれない。** エージェントは以下を実行しない。

### 公開ゲート（人間）

- [ ] GPT で画像を生成し、`medium/images/YYYY-MM/` に保存して内容を確認した
- [ ] **人間が記事全文を読み、自分の言葉として責任を持てるよう加筆修正した**
- [ ] Medium の AI コンテンツポリシーを**公式ページで再確認**し（`.claude/rules/platforms/medium.md` の「最重要の制約」参照）、開示文言の要否・paywall 可否を判断した。確認できない場合は公開しない
- [ ] 投稿先 Publication のガイドライン（AI 記事の扱い）を確認した

### 公開作業

1. frontmatter を除去し、Markdown をリッチテキスト化して Medium エディタに貼り付ける（**raw Markdown を直接貼り付けない**。手順は `.claude/rules/platforms/medium.md` の「公開モデル」）
2. 貼り付け結果を目視確認する（見出しレベル・リンク・強調・画像）。コードブロックは Medium エディタ上で作り直し、言語指定・Pullquote・キャプションを整える
3. タイトル・サブタイトル・タグ（5 件）を設定する
4. Story settings で SEO title / description・feature image を確認する（クロスポストなら canonical も）
5. Publication に投稿する場合は submit し、承認を待つ
6. 公開する

## 8. 公開後の監視

| 項目 | 頻度 | 内容 |
| --- | --- | --- |
| Claps / Responses / Follower | 公開後 1 週間 | 読者フィードバックの確認 |
| Medium Stats（views / reads / read ratio） | 月次 | Read ratio 50% 未満なら構成を見直す |
| AI 検索での引用 | 四半期 | ChatGPT Search / Perplexity AI での引用有無を確認 |
| 内容の陳腐化 | 四半期 | バージョン・数値の更新要否。修正はローカル → Medium の順 |
