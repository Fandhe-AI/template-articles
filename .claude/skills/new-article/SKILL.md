---
name: new-article
description: 新しい記事のディレクトリ構造を作成し、Phase 1（リサーチ）を開始する
argument-hint: "<article-name> (kebab-case, 例: geo-seo-optimization-guide)"
user-invocable: true
---

新しい記事 `$ARGUMENTS` のディレクトリ構造を作成し、リサーチフェーズを開始する。

## 手順

1. 記事名を検証する
   - kebab-case であること（英数字とハイフンのみ）
   - `articles/$ARGUMENTS/` がまだ存在しないこと
   - 引数が空の場合はユーザーに名前を聞く

2. ディレクトリ構造を作成する:
   ```
   articles/$ARGUMENTS/
   └── README.md
   ```

3. `README.md` を以下の内容で作成する:
   ```markdown
   # $ARGUMENTS

   ## ステータス

   | フェーズ | 状態 | 担当エージェント | 更新日 |
   |---------|------|----------------|--------|
   | 1. リサーチ | 🔄 進行中 | strategist-researcher | <今日の日付> |
   | 2. アウトライン | ⬜ 未着手 | strategist-researcher | - |
   | 3. 執筆 | ⬜ 未着手 | module-creator | - |
   | 4. レビュー | ⬜ 未着手 | auditor | - |
   | 5. テクニカル | ⬜ 未着手 | technical-translator | - |

   ## 概要
   <!-- 一言で記事の概要を説明 -->

   ## ドキュメント
   - [リサーチ](./01-research.md)
   ```

4. ユーザーにディレクトリが作成されたことを伝え、Phase 1（リサーチ）を開始する
   - `_templates/01-research.md` のテンプレートを参照しながら進める
   - `.claude/rules/phases/01-research-prompt.md` のプロンプト構造に従う
   - まずユーザーにターゲットキーワードと記事の背景を聞く
