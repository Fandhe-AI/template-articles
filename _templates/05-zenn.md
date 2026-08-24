# Phase 5（Zenn）: Zenn 公開変換

- 記事: `<article-name>`
- 担当エージェント: zenn-publisher
- 更新日: YYYY-MM-DD
- 出力先: `zenn/articles/<slug>.md`

## 1. slug

| 項目 | 値 |
| --- | --- |
| slug | `yyyy-mm-topic-name` |
| 公開 URL | `https://zenn.dev/<publication\|user>/articles/<slug>` |
| 文字数 | XX / 50 |

<!-- slug は公開後に変更不可。確定前にユーザー確認を取ったか？ -->

## 2. Frontmatter

```yaml
---
title: ""
emoji: ""
type: "tech"
topics: []
published: false
---
```

### 設計根拠

| フィールド | 値 | 根拠 |
| --- | --- | --- |
| title | | Phase 2 のタイトル案 X を採用。理由: |
| emoji | | 候補: 🅰 / 🅱 / 🅲 → ユーザー選択 |
| type | | 判定理由: |
| topics | | Phase 1 の優先度「高」エンティティから選定。Zenn 既存トピック名との対応: |

## 3. Zenn 記法への変換

| 箇所 | 変換前 | 変換後 | 理由 |
| --- | --- | --- | --- |
| セクション X | 注意書きの段落 | `:::message alert` | |
| セクション Y | 手順の図（画像） | Mermaid | AI が読み取れるテキスト形式 |
| コードブロック全体 | ```` ```ts ```` | ```` ```ts:path/to/file.ts ```` | ファイル文脈の明示 |
| 引用元 | 単独行 URL | インラインリンク | 主張と引用元を結び付ける |
| 見出し | H1 あり | 削除（frontmatter title が H1） | |
| 目次 | 手動目次 | 削除 | Zenn が H2/H3 から自動生成 |

### `:::details` の使用箇所

<!-- 結論・要点を折りたたんでいないか必ず確認する -->

| 箇所 | 内容 | 折りたたんで良い理由 |
| --- | --- | --- |
| | | |

## 4. 画像

| ファイル | 配置先 | Alt text |
| --- | --- | --- |
| | `zenn/images/YYYY-MM/<name>.png` | |

## 5. Lint 結果

```bash
cd zenn && pnpm run lint
```

| ツール | 指摘件数 | 対応 |
| --- | --- | --- |
| textlint | | |
| markdownlint | | |

### 未対応の指摘（意味が変わるため人間の判断が必要）

| 箇所 | 指摘 | 提案 |
| --- | --- | --- |
| | | |

## 6. Phase 5 完了チェックリスト

**Phase 5 は「公開可能な状態を作る」までで完了する。** `published: false` のまま完了してよい。公開そのものは Phase 5 の完了後に人間が行う（セクション 7 を参照）。

### 変換の妥当性

- [ ] slug が 12〜50 文字、半角英小文字・数字・ハイフン・アンダースコアのみ
- [ ] `topics` が 5 件以内
- [ ] `emoji` が 1 文字（合字でない）
- [ ] `type` が `tech` / `idea` のいずれか
- [ ] H1 を使用していない（見出しは H2 起点。本文の `#` は markdownlint の MD025 で検出される）
- [ ] 手動の目次がない
- [ ] `:::details` に結論・要点を入れていない
- [ ] コードブロックにファイル名が付いている
- [ ] `published` が `false` である（Phase 5 完了時点では `false` が正しい）
- [ ] `publication_name` は Publication に投稿する場合のみ記載している（空文字を残していない）
- [ ] `published_at` は予約投稿する場合のみ記載している（`published: false` のまま残していない）

### コンテンツ

- [ ] 全リンクが有効（404 なし）
- [ ] 全画像に説明的な Alt text がある
- [ ] 画像パスが `/images/` から始まる
- [ ] `cd zenn && pnpm run lint` が通る（または未対応の理由を記録）

### プレビュー

- [ ] `cd zenn && pnpm run preview` で表示崩れがない
- [ ] Mermaid 図が正しくレンダリングされる

<!-- 上記が全て埋まれば Phase 5 は完了。/advance-phase の完了判定はここまでを見る。 -->

## 7. 公開手順（Phase 5 完了後、人間が実施）

**このセクションは Phase 5 の完了条件に含まれない。** 公開は人間の判断で、任意のタイミングで行う。エージェントは以下を実行しない。

### 公開ゲート（人間）

- [ ] **人間が記事全文を読み、自分の言葉として責任を持てる内容であることを確認した**
- [ ] Zenn の生成 AI による記事量産禁止ポリシーに抵触しないことを確認した

```bash
# 公開（人間の最終確認後）
# zenn/articles/<slug>.md の published を true に変更してから
git add zenn/articles/<slug>.md
git commit -m "post: <slug>"
git push origin <Zenn 連携ブランチ>
```

## 8. 公開後の監視

| 項目 | 頻度 | 内容 |
| --- | --- | --- |
| Zenn のいいね・コメント | 公開後 1 週間 | 読者フィードバックの確認 |
| AI 検索での引用 | 四半期 | ChatGPT Search / Perplexity AI での引用有無を確認 |
| 内容の陳腐化 | 四半期 | バージョン・数値の更新要否を確認 |
