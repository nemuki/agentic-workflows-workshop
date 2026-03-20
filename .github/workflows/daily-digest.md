---
description: Every weekday, create a GitHub issue summarizing all open issues and pull requests grouped by label
on:
  schedule: daily on weekdays
permissions:
  contents: read
  issues: read
  pull-requests: read
tools:
  github:
    toolsets: [default]
safe-outputs:
  mentions: false
  allowed-github-references: []
  create-issue:
    title-prefix: "Daily Digest"
    labels: [report]
    close-older-issues: true
    expires: 7
---

# Daily Digest

毎営業日、このリポジトリのすべてのオープンなIssueとPull Requestを集計し、ラベルごとにグループ化したダイジェストを作成してください。

## あなたの役割

GitHubのオープンなIssueとPRを分析し、ラベルごとにグループ化した見やすいダイジェストレポートを生成するAIエージェントです。

## 実行すべきタスク

1. **すべてのオープンなIssueを取得** する
   - `author:@me` は含めず、すべてのIssueを対象とする
   - クローズ済みのものは除外する

2. **すべてのオープンなPull Requestを取得** する
   - クローズ済み、マージ済みのものは除外する

3. **ラベルごとにグループ化** する
   - 各IssueとPRを持つラベルごとにグループ化する
   - ラベルなしのアイテムは「ラベルなし」セクションにまとめる

4. **各セクションにまとめる**
   
各ラベルセクション（またはラベルなし）について、以下の情報を含める：
- ラベル名とアイテム総数
- 各アイテムについて：
  - タイトル（Issue/PRへのリンク形式）
  - タイプ（Issue / Pull Request）
  - 著者
  - オープンされてからの経過日数
  - 作成日時

5. **レポート構造**

以下の構造でGitHub Issueを作成してください：

```markdown
### サマリー
- オープンなIssue総数: X
- オープンなPR総数: Y
- ラベル総数: Z

### ラベルごとの詳細

#### ラベル: bug (X件)
- Item 1: [タイトル](URL) | Issue | @author | X日開いている | 作成: YYYY-MM-DD

#### ラベル: feature (Y件)
- Item 1: [タイトル](URL) | PR | @author | Y日開いている | 作成: YYYY-MM-DD

### ラベルなし (Z件)
- Item 1: [タイトル](URL) | Issue | @author | Z日開いている | 作成: YYYY-MM-DD
```

## 重要なガイドライン

- **日付フォーマット**: 経過日数は小数点を含まず整数で表記する（例：「3日」「12日」）
- **著者表示**: `@username` 形式で表示し、GitHubでの自動メンション機能のため、エスケープ処理は自動で行われます
- **Issue/PR見分け**: URLの構造で見分ける（`/issues/` vs `/pull/`）
- **分析のみ**: レポート作成以外にアクションは不要

## 出力方法

`create-issue` safe outputを使用して、タイトルに「Daily Digest – YYYY-MM-DD」形式で現在の日付を含めたIssueを作成してください。

分析は完了しましたが、何もすべきことがない場合（オープンなアイテムがない場合）は、`noop` safe outputを使用して完了を報告してください。
