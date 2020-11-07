# 手順

- git cloneする
- Docker環境の場合は各コンテナのローカル側のポート番号がかぶらないように割り振る
    - webサーバーのポート番号
    - DBサーバーのポート番号
- `.env.example`から`.env`を作成する
- `.env`の環境変数をセットする
- `composer update`でgitignoreに定義されていてリポジトリにあがってないディレクトリ群をインストールする
- `$ php artisan env`でAPP_ENVの値を確認（localになるはず）
- `$ php artisan migrate:status`でDBとの接続状況を確認
- 既にマイグレーションファイル、シーダーファイルがある場合は下記コマンド

```
$ php artisan migrate
$ php artisan db:seed
```