# Template
新規プロジェクト用のテンプレートリポジトリ。Node.js + Jest によるテスト、GitHub Actions による CI/CD、Issue/PR テンプレート、Dependabot をあらかじめ設定済み。

## セットアップ
```bash
npm install
```

## テスト
```bash
npm test
```

## 構成
### サンプルコード（実装に合わせて削除・置き換える）
- `sum.js` — Jest の動作確認用サンプル実装
- `sum.test.js` — 上記に対応するサンプルテスト

### 雛形（そのまま利用する土台）
- `package.json` — npm scripts・依存関係の定義
- `.github/workflows/ci.yml` — main への push・PR 時にテストを実行
- `.github/workflows/cd.yml` — main への push 時に本番デプロイ（`deploy.sh` を用意して利用）
- `.github/workflows/sync-labels.yml` — `labels.json` の内容を GitHub のラベルへ自動反映
- `.github/dependabot.yml` — 依存関係の自動更新
- `.github/ISSUE_TEMPLATE/` — バグ報告・機能要望・調査用の Issue テンプレート
- `.github/pull_request_template.md` — PR テンプレート
- `labels.json` — プロジェクト共通のラベル定義（種別・PR・Issue・タスク分解・コメント・ステータスの6カテゴリ）

## 使い方
1. このリポジトリを新規プロジェクトの雛形として使用する
2. `sum.js` / `sum.test.js` などのサンプルコードを実際の実装・テストに置き換える
3. 雛形部分（CI/CD・Issue/PR テンプレート・Dependabot 設定）はそのまま利用する
4. GitHub の「Actions」タブから `Sync Labels` ワークフローを手動実行（Run workflow）し、`labels.json` に定義されたラベルを反映する
   - 以後は `labels.json` を編集して `main` に push すると自動で同期される

## ラベル管理について
GitHubのラベルは、リポジトリを作るたびに手動で作り直すのが面倒になりがちです。このテンプレートでは [labels-config](https://zenn.dev/ait/articles/zenn-labels-config-guide) というツールを使い、`labels.json` に定義した内容をコマンド一つ（またはpush時に自動で）GitHubへ反映できるようにしています。

まだ導入したことがない方向けに、最低限の使い方を紹介します。

### 導入手順
```bash
# 1. GitHub CLI のセットアップ（未導入の場合）
gh auth login

# 2. labels-config のインストール
npm install -g @asagiri-design/labels-config

# 3. 動作確認
labels-config --version
```

### ローカルで手動同期する場合
```bash
# プロジェクトのディレクトリで実行
labels-config sync --owner {owner} --repo {repo} --file labels.json --dry-run   # 事前確認
labels-config sync --owner {owner} --repo {repo} --file labels.json --delete-extra   # 反映
```

`--owner`・`--repo`を毎回指定するのが面倒な場合は、記事内で紹介されているシェル関数（`labels-sync`）を導入すると、現在のディレクトリから自動でリポジトリ情報を取得してくれます。詳しくは[記事](https://zenn.dev/ait/articles/zenn-labels-config-guide) を参照してください。

### GitHub Actionsで自動化する場合
本テンプレートには `.github/workflows/sync-labels.yml` を同梱済みです。`labels.json` を編集して `main` に push するだけで、GitHub Actions が自動でラベルを同期します。初回のみ、Actionsタブから `Sync Labels` を手動実行してください（詳細は「[使い方](#使い方)」参照）。

## ブランチ保護ルールの設定
このテンプレートから作成したリポジトリでは、CI/CD やレビューを確実に機能させるため、`main` ブランチに保護ルールを設定することを推奨します。
Template repository は Branch protection rules をコピーしないため、新規プロジェクトを作るたびに以下の手動設定が必要です。

1. リポジトリの **Settings** → **Branches** を開く
2. **Add branch protection rule** をクリック
3. Branch name pattern に `main` を入力
4. 以下の項目にチェック
   - Require a pull request before merging
   - Require approvals（1名以上）
   - Require status checks to pass before merging（`ci.yml` のチェックを指定。事前に一度 CI を実行しておく必要があります）
   - Do not allow bypassing the above settings
5. **Create** をクリックして保存

### （参考）GitHub CLI で設定する場合
```bash
gh api --method PUT /repos/{owner}/{repo}/branches/main/protection \
  -f "required_pull_request_reviews[required_approving_review_count]=1" \
  -F "enforce_admins=true" \
  -f "restrictions=null"
```

## GitHub Projectsで進捗を管理する
タスクの進捗をカンバン方式で共有します。

1. 「Projects」タブ → 「New project」→「Board」テンプレートで作成
2. Issueを追加し、「Todo」「In Progress」「Done」で管理

おすすめ：ビュー設定で「Group by: Assignees」にすると担当者ごとのスイムレーン表示になり、負荷の偏りが一目でわかります。
