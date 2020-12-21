# 手順

- Docker環境の場合は各コンテナのローカル側のポート番号がかぶらないように割り振る
    - webサーバーのポート番号
    - DBサーバーのポート番号
    - docker-compose build
    - docker-compose up -d
- git cloneする（リポジトリ名のディレクトリを作成しない場合は`$ git clone ****.git .`)
- `.env.example`から`.env`を作成する
- `.env`の環境変数をセットする(DBの設定)
- `composer update`でgitignoreに定義されていてリポジトリにあがってないディレクトリ群をインストールする
- APP_KEYの設定（php artisan key:generate)
- `$ php artisan env`でAPP_ENVの値を確認（localになるはず）
- `$ php artisan migrate:status`でDBとの接続状況を確認
- 既にマイグレーションファイル、シーダーファイルがある場合は下記コマンド

```
$ php artisan migrate
$ php artisan db:seed

// これでOK
$ php artisan migrate --seed
```

# エラー
DB接続系のエラー

```
// 以下のコマンドを実行するとMySQL接続エラー
$ php artisan migrate:status

SQLSTATE[HY000] [1045] Access denied for user 'unilive_user'@'172.21.0.3' (using password: YES) (SQL: select * from information_schema.tables where table_schema = unilive_admin and table_name = migrations and table_type = 'BASE TABLE')
```

解決方法
- `docker-compose.ymlと.envのDB設定のミスがないか確認`
    - docker-compose:MYSQL_USER
    - .env:DB_USERNAME
- `docker volumeを削除`
    - docker volumeを使っているDBを構築後、設定内容を変えて再構築すると元のvolumeと干渉してエラーになる
    - docker volume list
    - docker volune rm ボリューム名：指定volume削除
    - docker volume rm：全削除

再度接続
```
$ php artisan migrate:status

Migration table not found.
```