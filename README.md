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
- `.github/dependabot.yml` — 依存関係の自動更新
- `.github/ISSUE_TEMPLATE/` — バグ報告・機能要望・調査用の Issue テンプレート
- `.github/pull_request_template.md` — PR テンプレート

## 使い方

1. このリポジトリを新規プロジェクトの雛形として使用する
2. `sum.js` / `sum.test.js` などのサンプルコードを実際の実装・テストに置き換える
3. 雛形部分（CI/CD・Issue/PR テンプレート・Dependabot 設定）はそのまま利用する
