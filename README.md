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
