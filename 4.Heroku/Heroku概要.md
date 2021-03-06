# Heroku概要

## Herokuとは
>Heroku はアプリの構築、提供、監視、スケールに役立つクラウドプラットフォームで、アイデアを出してから運用を開始するまでのプロセスを迅速に進めることが可能です。また、インフラストラクチャの管理の問題からも解放されます。

（引用：https://jp.heroku.com/what）

Herokuを使用すると、Ruby、Node.js、Java、Python、Clojure、Scala、Go、PHPで記述されたアプリケーションをデプロイ、実行、管理できます。アプリケーションを実行するための環境。herokuはPaaS（Platform as a Service）と呼ばれる形態でサービスを利用できます。PaaSを簡単に言えばアプリの公開に必要な手順を代行してくれるサービスです。

インターネット上にアプリケーションを公開するために必要な手順

- サーバPCやルータなどのハードウェアを購入
- インターネットに接続し、ネットワークを構築
- サーバの仮想化環境を整備
- LinuxやWindowsサーバなどのOSをインストール
- Oracle、MySQL、PostgreSQLなどのデータベースをセットアップ
- Java、Ruby、PHPなどのアプリケーション実行環境をセットアップ

PaaSはこのようなプラットフォーム構築（アプリケーション公開）にかかる数々の作業を代行してくれるサービス。

## Herokuのメリット
### 拡張機能が豊富
1つ目は、「拡張機能が豊富」な点です。

開発を進めていく上で追加で環境が必要となるケースはよくあります。

たとえばアプリ開発を進めていくと、登録したユーザーにメールを送信する処理を作るケースが良くあります。ただメールを送信するためには、メール配信用の環境が必要です。

herokuでは拡張機能を使うだけで簡単にメール配信用の環境が利用できるようになります。このように、アプリ開発に必要な拡張機能が豊富なので、開発環境に煩わしさを感じる心配はなくなります！

### スケールアウトが簡単
2つ目は「スケールアウトが簡単」な点です。

スケールアウトとは、システムを構成するサーバーの台数を増やすことで、システムの処理能力を高める、つまり…コンピュータの環境を増強する方法の1つです。

アプリを開発して公開した後、スムーズに運用を進めるためには「最適な環境を維持すること」が必要不可欠です。

具体的に言うと、アプリを利用するユーザーの増加などに合わせて、アプリが滞りなく動くために、環境を増強する必要があるのです。つまりハードウェアの性能を上げるための、スケールアウトが重要となってきます。

この点herokuは、ぬかりありません。herokuのダッシュボード画面で操作したり、簡単なコマンドを実行するだけですぐにスケールアウトができるのです。

規模に合わせて最適な環境を維持できるのは嬉しいポイントですね。

### 運用をサポートする標準機能が揃っている
3つ目は、「運用をサポートする標準機能が揃っている」点です。

アプリは開発が終わったら、それで作業が終わるわけではありません。開発してアプリをリリースした後、アプリが正しく動くよう運用していく必要があります。その時に重要なのが、アプリが動く環境を正しく把握する技術です。

具体的に言うと、アプリの状態を確認する「ログ」や、サーバーの状態を確認する「パフォーマンスモニター」などをチェックし、状態を把握するスキルが必要です。

herokuでは、ログやパフォーマンスモニターなどを確認できる機能が備わっています。そのため、状態のチェックをするために別途用意する必要がありません。

またデータベースのデータを守るためのバックアップ機能もあるため、運用にかかる手間が減らせるだけでなく、運用にかかるコストも抑えられるのです。