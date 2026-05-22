# 同人誌 AUTO CREATOR — デプロイガイド

## このフォルダーの構成

```
deploy_package/
├── public/
│   └── index.html        ← メインのツール本体
├── api/
│   └── claude.js         ← APIキーを安全に隠すサーバー関数
├── vercel.json           ← Vercelデプロイ設定
├── .env.example          ← 環境変数のサンプル（コピーして使う）
├── .gitignore            ← GitHubに上げてはいけないファイルの設定
└── README.md             ← このファイル
```

---

## デプロイ手順（所要時間：約30分）

### STEP 1: 必要なアカウントを作成する

以下の3つのアカウントが必要です（すべて無料）:

1. **GitHub** → https://github.com
2. **Vercel** → https://vercel.com（GitHubアカウントでログイン可）
3. **Anthropic Console** → https://console.anthropic.com（APIキー取得用）

---

### STEP 2: APIキーを取得する

1. https://console.anthropic.com/settings/keys にアクセス
2. 「Create Key」をクリック
3. 名前を入力（例：doujinshi-tool）して「Create Key」
4. 表示されたキー（`sk-ant-api03-...`）をメモ帳にコピーして保存
   ⚠️ このキーは一度しか表示されません！必ず保存してください

---

### STEP 3: GitHubにアップロードする

**方法A：ブラウザで直接アップロード（簡単）**

1. github.com にログイン
2. 右上の「+」→「New repository」をクリック
3. Repository name に「doujinshi-creator」と入力
4. 「Create repository」をクリック
5. 「uploading an existing file」のリンクをクリック
6. このフォルダー内のファイルをすべてドラッグ&ドロップ
7. 「Commit changes」をクリック

**方法B：コマンドラインを使う場合**

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/あなたのユーザー名/doujinshi-creator.git
git push -u origin main
```

---

### STEP 4: Vercelにデプロイする

1. https://vercel.com にアクセス（GitHubアカウントでログイン）
2. 「Add New...」→「Project」をクリック
3. 「doujinshi-creator」リポジトリの「Import」をクリック
4. 設定はそのままで「Deploy」をクリック
5. ⏳ 1〜2分待つと自動でデプロイ完了！
6. `https://doujinshi-creator-xxx.vercel.app` のようなURLが発行されます

---

### STEP 5: APIキーを環境変数に設定する（重要！）

1. Vercelのダッシュボードで「doujinshi-creator」プロジェクトを開く
2. 「Settings」タブ → 「Environment Variables」をクリック
3. 以下を入力して「Save」:
   - Name: `ANTHROPIC_API_KEY`
   - Value: `sk-ant-api03-...`（STEP 2でコピーしたキー）
4. 「Deployments」タブ → 最新のデプロイを選択 → 「Redeploy」をクリック

---

### STEP 6: 動作確認

発行されたURLにアクセスして、ジャンルを選択 → 生成開始 → 正常に動けば完成！

---

## 費用の目安

| 項目 | 費用 |
|------|------|
| Vercel（ホスティング） | 無料 |
| GitHub | 無料 |
| Anthropic API | 100P生成1回 ≈ 50〜200円（従量課金） |
| 独自ドメイン（任意） | 年1,000〜3,000円 |

### APIの使いすぎを防ぐ設定

https://console.anthropic.com/settings/billing で「Usage Limits」から月額上限を設定できます（例：$20/月）。

---

## よくあるトラブル

**Q. デプロイ後にAPIが動かない**
→ STEP 5の環境変数の設定ができているか確認。設定後にRedeploy必須。

**Q. 「API 500エラー」が出る**
→ APIキーが間違っているか、Anthropicのクレジットが不足している可能性があります。

**Q. ページが表示されない**
→ vercel.json の routes 設定を確認してください。

---

## セキュリティについて

- APIキーは `.env.local` または Vercel の環境変数にのみ保存する
- `.env.local` は `.gitignore` に含まれているのでGitHubにはアップされない
- `api/claude.js` がAPIキーをサーバーサイドで処理するため、ブラウザからは見えない
