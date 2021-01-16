# HerokuへのLaravelアプリデプロイ

- Herokuに登録する
- Herokuでappを作成
- Heroku CLIのインストール
- GitHubと連携
- procfileを作成（プロジェクト直下）
- PostgreSQLの用意
- buildpackの設定
- 環境変数の設定
- package.jsonに設定を追記
- circle ciのconfigファイル変更

## Herokuに登録する
クレジットカードを登録すると月に使える時間が伸びる

## Herokuでappを作成
一意の名前じゃないとダメ。<br>
https://mensetsu-app.herokuapp.com/

## Heroku CLIのインストール
```
$ brew tap heroku/brew && brew install heroku
```

## GitHubと連携
Deployment methodをGitHubに設定する。連携したGitHubのリモートリポジトリを選択。Automatic deploysをEnableにする。<br>
Wait for CI to pass before deployはCIツールを使うときはチェック

## procfileを作成（プロジェクト直下）
```
web: vendor/bin/heroku-php-apache2 public/
```

意味は調べる

## PostgreSQLの用意
ResourcesタブからHeroku Postgresを選択する（Hobby Dev）

## Buildpacksの設定
SettingsタブのBuildpacks

- Laravelオンリー：heroku/php
- Vueも使う：heroku/nodejs

それぞれが何をしてくれているのかは調べる

## Herokuに環境変数の設定

ポスグレの環境変数はこのコマンドで見れる

```
$ heroku pg:credentials:url -a mensetsu-app

Connection information for default credential.
Connection info string:
   "dbname=d1td59no8t0hfb host=ec2-54-146-142-58.compute-1.amazonaws.com port=5432 user=xlpmrjkszonscc password=c2d738b6bb53d76a921e1c472ea2105d7d12e827158c3c18048c2bb6ea0da72b sslmode=require"
Connection URL:
   postgres://xlpmrjkszonscc:c2d738b6bb53d76a921e1c472ea2105d7d12e827158c3c18048c2bb6ea0da72b@ec2-54-146-142-58.compute-1.amazonaws.com:5432/d1td59no8t0hfb
```

APP_KEYは以下のコマンド
```
heroku config:set APP_KEY=$(php artisan --no-ansi key:generate --show)
```

他に設定した環境変数
```
APP_NAME=Laravel
APP_ENV=production
APP_DEBUG=false
APP_URL=https://mensetsu-app.herokuapp.com/

DB_CONNECTION=pgsql
DB_HOST=ec2-54-146-142-58.compute-1.amazonaws.com
DB_PORT=5432
DB_DATABASE=d1td59no8t0hfb
DB_USERNAME=xlpmrjkszonscc
DB_PASSWORD=c2d738b6bb53d76a921e1c472ea2105d7d12e827158c3c18048c2bb6ea0da72b
```
↑herokuのサイトのOverview>Postgresのボタン>Setting>View Credentials...からでも見れる

他に
AWS、Google API、Twitter API用の環境変数も

## GitHubにpush
```git 
$ git push origin master or feature/branch
```
## migration

```
$ heroku run "php artisan migrate" -a mensetsu-app
```

シーディング

```
$ heroku run "php artisan db:seed" -a アプリ名
```

## package.jsonに設定を追記

追記（理由は調べる）
```json
    "Dependencies": {
        "axios": "^0.19",
        "cross-env": "^5.1",
        "laravel-mix": "^4.0.7",
        "lodash": "^4.17.13",
        "resolve-url-loader": "^2.3.1",
        "sass": "^1.15.2",
        "sass-loader": "^7.1.0",
        "vue": "^2.6.11",
        "vue-template-compiler": "^2.6.11"
    }
```

scriptに追記（理由は調べる）
```json
        "heroku-prebuild": "npm run production",
```

## Postgresのクライアントツール

Table Plus

## circle ciのconfigファイル変更

```yml
jobs:
  build:
    executor:
      name: laravel-circleci
    steps:
      - checkout
      - install-dockerize
      - install-php-extensions
      - restore-cache-composer
      - install-composer
      - save-cache-composer
      - restore-cache-npm
      - npm-ci
      - save-cache-npm
      - npm-run-dev
      - migration
      - test-unittest
  deploy:
    docker:
      - image: circleci/php:7.4-node-browsers
    steps:
      - checkout
      - run:
          name: heroku deploy
          command: |
            git push https://heroku:$HEROKU_API_KEY@git.heroku.com/$HEROKU_APP_NAME.git master

workflows:
  version: 2
  build_deploy:
    jobs:
      - build
      - deploy:
          requires:
            - build
          filters:
            branches:
              only:
                - master

```

なぜかエラー発生

```
remote: -----> Prebuild        
remote:        Running heroku-prebuild        
remote:                
remote:        > @ heroku-prebuild /tmp/build_f58ff4ae        
remote:        > npm run production        
remote:                
remote:                
remote:        > @ production /tmp/build_f58ff4ae        
remote:        > cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js        
remote:                
remote: sh: 1: cross-env: not found        
remote: npm ERR! code ELIFECYCLE        
remote: npm ERR! syscall spawn        
remote: npm ERR! file sh        
remote: npm ERR! errno ENOENT        
remote: npm ERR! @ production: `cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js`        
remote: npm ERR! spawn ENOENT        
remote: npm ERR!         
remote: npm ERR! Failed at the @ production script.        
remote: npm ERR! This is probably not a problem with npm. There is likely additional logging output above.        
remote: 
remote: npm ERR! A complete log of this run can be found in:        
remote: npm ERR!     /tmp/npmcache.kdG7G/_logs/2020-10-12T14_54_28_712Z-debug.log        
remote: npm ERR! code ELIFECYCLE        
remote: npm ERR! errno 1        
remote: npm ERR! @ heroku-prebuild: `npm run production`        
remote: npm ERR! Exit status 1        
remote: npm ERR!         
remote: npm ERR! Failed at the @ heroku-prebuild script.        
remote: npm ERR! This is probably not a problem with npm. There is likely additional logging output above.        
remote: 
remote: npm ERR! A complete log of this run can be found in:        
remote: npm ERR!     /tmp/npmcache.kdG7G/_logs/2020-10-12T14_54_28_733Z-debug.log        
remote: 
remote: -----> Build failed        
remote:                
remote:        We're sorry this build is failing! You can troubleshoot common issues here:        
remote:        https://devcenter.heroku.com/articles/troubleshooting-node-deploys        
remote:                
remote:        Some possible problems:        
remote:                
remote:        - Node version not specified in package.json        
remote:          https://devcenter.heroku.com/articles/nodejs-support#specifying-a-node-js-version        
remote:                
remote:        Love,        
remote:        Heroku        
remote:                
remote:  !     Push rejected, failed to compile Node.js app.        
remote: 
remote:  !     Push failed        
remote: Verifying deploy...
remote: 
remote: !	Push rejected to ************.        
remote: 
To https://git.heroku.com/************.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'https://heroku:************************************@git.heroku.com/************.git'

Exited with code exit status 1
CircleCI received exit code 1
```

解決方法
package.jsonにこの2行を記載

```json
        "heroku-prebuild": "npm install --global cross-env",
        "heroku-postbuild": "npm run production"
```
cross-envが見つからないからインストール。今後ちゃんと調べないといけない。

Herokuの公式ドキュメントに実行順序が書いている

Heroku-specific build steps
While npm install and yarn install have standard preinstall and postinstall scripts, you may want to run scripts only before or after other Heroku build steps. For instance, you may need to configure npm, git, or ssh before Heroku installs dependencies, or you may need to build production assets after dependencies are installed.

For Heroku-specific actions, use the heroku-prebuild, heroku-postbuild, and heroku-cleanup scripts:

```
"scripts": {
  "heroku-prebuild": "echo This runs before Heroku installs dependencies.",
  "heroku-postbuild": "echo This runs after Heroku installs dependencies, but before Heroku prunes and caches dependencies.",
  "heroku-cleanup": "echo This runs after Heroku prunes and caches dependencies."
}
```