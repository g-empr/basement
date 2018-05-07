# Nuxt.js + firebase hostingまでにやったことメモ
## 環境を整える
- Node.js
    - npm
    - nodist
    - firebase-tools
- Vue.js
    - vue-cli
- Firebase
  
## Node.js/npmをインストール
nodistは[ここ](https://github.com/marcelklehr/nodist/releases)から。
```
nodist 10.0.0
```
```
nodist npm 6.0.0
```
`vue`、`vue-cli`や`firebase-tools`をインストールしておく。
```
npm install -g vue vue-cli firebase-tools
```
## Nuxt.jsのプロジェクトを作成
Nuxtコミュニティのリポジトリからスターターを取り込む。
```
vue init nuxt-community/starter-template the-base
```
その後、プロジェクトフォルダに入りモジュールをインストール。Nuxtの立ち上がりも確認しておく。
```
cd the-base
npm install
```
```
npm run dev
```
## デプロイの準備
firebaseにデプロイするために`firebase.json`を用意する。
```
{
    "hosting": {
        "public": "dist",
        "ignore": [
        "firebase.json",
        "**/.*",
        "**/node_modules/**"
        ]
    }
}
```
## ぶち当たったところ
### Gitが認証エラー―を履くようになった
→ 最新版(ここでは2.17.0)を入れたら改善。
### npm run dev でエラーを吐く
→ node/npmのバージョンを更新したら改善。
### 
