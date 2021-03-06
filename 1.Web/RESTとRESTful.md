# REST

- REpresentational State Transferの略
- HTTPメソッドを使って決まったルールでアプリケーションにアクセスできるようにする考え方
- RESTなWebサービスは、そのサービスのURIにHTTPメソッドでアクセスすることでデータの送受信を行う
- RESTは設計に際し以下を設計原則とするよう提言されている

    1. アドレス指定可能なURIで公開されていること
    2. インターフェース(HTTPメソッドの利用)の統一がされていること
    3. ステートレスであること（サーバーはセッション情報を保持しない）
    4. 処理結果がHTTPステータスコードで通知されること

- RESTで用いられるHTTPメソッドは下記のようにCRUD処理と対応付けられている
- 登録：POST：CREATE
- 取得：GET：READ
- 更新：PUT：UPDATE
- 削除：DELETE：DELETE

# RESTful

- RESTという考え方に基づいて設計されているプログラムは「RESTfulである」と表現される
- RESTfulなサービスは情報の取得や送信など基本的な操作の方法が統一されている→他のプログラムから簡単にアクセスしてサービスを利用できる

## RESTful API

- `YouTube API`のような外部アプリケーションとの接続のルール

