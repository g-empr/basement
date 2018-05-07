# Nuxt.js + firebase hostingまでにやったことメモ
## 環境を整える
- Node.js
    - npm
    - nodist
    - firebase-tools
- Vue.js
    - vue-cli
- Firebase
- CircleCI
  
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
firebaseにデプロイするために`firebase.json`を用意し、プロジェクト内に配置。
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
firebaseにログインしておく。
```
firebase login
```
予めブラウザからfirebase consoleにアクセスし、hosting用のプロジェクトを新規作成しておく。  
できていれば、コマンドにてデプロイ先を指定する。
```
firebase use --add [firebaseのプロジェクトID]
```
最後にデプロイ。
```
firebase deploy
```
## CircleCIと連携する
これを行うことで`git push`した際にデプロイも行うようになる。  
予めGitHubアカウントで[CircleCI](https://circleci.com/)にログインし、プロジェクトの登録を行う必要がある。  
  
まずデプロイするためのトークンを取得。
```
firebase login:ci
```
次に`.circleci/config.yml`を作成し、以下を記述。
```
version: 2
jobs:
  build:
    working_directory: ~/repo
    docker:
      - image: circleci/node:8.9.4
    steps:
      - checkout
      - run:
          name: Install firebase-tools
          command: |
            curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
            sudo apt-get install -y nodejs
            echo prefix=${HOME}/.local >> ~/.npmrc
            npm install -g firebase-tools
      - restore_cache:
          keys:
          - v1-dependencies-{{ checksum "package.json" }}
          - v1-dependencies-
      - run: npm install
      - save_cache:
          paths:
            - node_modules
          key: v1-dependencies-{{ checksum "package.json" }}
      - run:
          name: Build
          command: npm run generate
      - run:
          name: Deploy
          command: ~/.local/bin/firebase deploy --token [さきほど取得したトークン] --project basement-12054
```
最後にpushして確認する。
```
git push
```
これでpush時にデプロイも行うようになる。
  
## ぶち当たったところ
### Gitが認証エラー―を履くようになった
→ 最新版(ここでは2.17.0)を入れたら改善。
### npm run dev でエラーを吐く
→ node/npmのバージョンを更新したら改善。
### CircleCIについて
→ 今回はじめて触ったところなので今後は要学習。結構便利だと思った。