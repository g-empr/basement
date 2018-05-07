# Nuxt.js + firebase hostingまでにやったことメモ
## 環境を整える
- Node.js
    - npm
    - nodist
    - firebase-tools
- Vue.js
    - vue-cli
- Firebase
  
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
最新版(ここでは2.17.0)を入れたら改善。
### npm run dev でエラーを吐く
node/npmのバージョンを更新したら改善。
### 
