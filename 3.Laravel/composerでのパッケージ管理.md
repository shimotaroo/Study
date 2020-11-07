# コマンド

```
バージョン管理
$ composer --vsersion
$ composer -V

インスtール
$ composer install

アップデート
$ composer update
```

# 各ファイルの役割

- composer.lock：現在使用しているパッケージのバージョン等が管理されます。
- composer.json：必要となるパッケージを記述します。
- composer.pharで実行。

# コマンドの使い分け

- composer install：composer.lockに書かれている各ライブラリをインストールする。
- composer update：composer.jsonをもとに各ファイルを最新版にアップデートする。

# 実際に使う場面

- 新しい環境ではじめにインストールするとき：composer install
- 何か新しいパッケージを追加したい：composer.jsonにかいてcomposer update
- 本番のライブラリを最新版にしたい：開発環境でcomposer updateして問題なければcomposer.lockファイルを本番にコピーしてcomposer intallする

共同開発で誰かだ新しくパッケージを追加した場合は`composer.json`にパッケージが書かれているので、それを`git pull`してターミナルで`composer update`すればローカルに反映される

[composer install と composer updateの違い](https://qiita.com/YusukeHigaki/items/47dd3ec23544225f7301)