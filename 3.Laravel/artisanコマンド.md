# artisanコマンド

.envのAPP_ENVを表示（デフォルトはlocal）

    $ php artisan env

DBとの接続状況を確認

    $ php artisan migrate:status

コントローラー作成

    $ php artisan make:controller ArticleController

コントローラー作成（主要7アクションを自動的に生成）

    $ php artisan make:controller ArticleController --resource

シングルアクションコントローラー作成

    $ php artisan make:controller ArticleController --invokable

マイグレーションファイル作成（テーブル作成）

    $ php artisan make:migration create_articles_table —-create=articles

マイグレーションファイル作成（カラム追加等変更）

    $ php artisan make:migration add_column_articles_table —-table=articles

マイグレーション実行

    $ php artisan migrate 

現在のテーブルを全削除してマイグレーションやり直し

     $ php artisan migrate:fresh

マイグレーション実行＆シーディング実行

    $ php artisan migrate --seed

マイグレーションロールバック（最後に実行したマグレーションを元に戻す）

    $ php artisan migrate:rollback

全てのマイグレーションをロールバックする

    $ php artisan migrate:reset

シーディング実行

    $ php artisan db:seed

特定のシーダーファイルをシーディング（外部キー制約があるとこっちの方しか使えないかも）

    $ php artisan db:seed --class=UsersTableSeeder

シーダーファイル作成

    $ php artisan make:seeder UserSeeder
    （UserTableSeederにする場合もアリ）

モデル作成

    $ php artisan make:model Article モデルの作成

モデルとファクトリーを同時に作る

    $ php artisan make:model Article —-factory

ルーティングの一覧を表示

    $ php artisan route:list

フォームリクエスト作成（バリデーションに使用）　※他にも使い道ある？

    $ php artisan make:request ArticleRequest フォームリクエスト作成（バリデーション等）

ポリシー作成

    $ php artisan make:policy ArticlePolicy --model=Article

Mailableクラスを継承したBareMailクラスを作る（メールを取り扱うクラス）

    $ php artisan make:mail BareMail

通知クラス作成

    $ php artisan make:notification PasswordResetNotification 通知クラスを作成

ファクトリー作成（モデルと対応させる）

    $ php artisan make:factory ArticleFactory —-model=Article

テスト作成

    $ php artisan make:test ArticleControllerTest

tinker起動

    $ php artisan tinker