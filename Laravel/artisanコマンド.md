# artisanコマンド

コントローラー作成

    $ make:controller ArticleController


マイグレーションファイル作成

    $ make:migration create_articles_table —-create=articles

マイグレーション実行

    $ migrate マイグレーション実行

モデル作成

    $ make:model Article モデルの作成

モデルとファクトリーを同時に作る

    $ make:model Article —-factory　モデルとfactoryを同時に作る

ルーティングの一覧を表示

    $ route:list

フォームリクエスト作成（バリデーションに使用）　※他にも使い道ある？

    $ make:request ArticleRequest フォームリクエスト作成（バリデーション等）

ポリシー作成

    $ make:policy ArticlePolicy --model=Article　ポリシーの作成

Mailableクラスを継承したBareMailクラスを作る（メールを取り扱うクラス）

    $ make:mail BareMail

通知クラス作成

    $ make:notification PasswordResetNotification 通知クラスを作成

ファクトリー作成（モデルと対応させる）
    $ make:factory ArticleFactory —-model=Article

テスト作成
    $ make:test ArticleControllerTest