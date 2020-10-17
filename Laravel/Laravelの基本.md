# Laravel基本
## MVC
※：前提Laravelはサーバ上で動く。

ユーザがブラウザ（BROWSER）を開いてウェブアプリケーションを閲覧したい時
- ユーザから「アプリ表示をお願いします！」と指令を出す行為がHTTPリクエスト
- サーバ側が「アプリ表示はコレです、どうぞ！」とページ表示内容を返す行為がHTTPレスポンスです

サーバー側（Laravel上）で動作する物

- Router（ルータ）
- Mocel（モデル）
- Controller(コントローラ)
- View（ビュー）

## Router

- ブラウザから来たリクエストを「どの処理（Controllerなど）へ送るか」を判断する交通整理的な役割
- Routerの機能がなければ、いくら素晴らしいアプリケーションを作ったとしても、アプリケーションの処理自体が始まらない

## Model

- MVCにおける「データベース」関連の処理を担当する部分
- 例えば、アプリケーションに新規登録するユーザの情報を保存する際に、このモデルを通じて情報がデータベースに保存される

## Controller

- Laravelにおける実行係
- Controllerがサーバ上のアプリケーションの機能実行を担う
- Routerに指定されたControllerが特定の処理を行い、Modelを操作して、新規ユーザ作成や保存処理などを行う

## View

- 表示機能
- 端的にいうとViewはWebページそのものと捉えてOK
- Controllerが処理した結果をもとに、Webページを出力して最終的にユーザに見せる役割
- Viewがないとユーザは何も情報を取得できない

## Router、Model、Controller、Viewに分ける理由

- 誰が見てもわかりやすいコードになるから
- 仮に細かい命令が増えてしまったとしても、これらModel/Router/Controller/Viewの役割さえ理解していれば、どのコードがどの命令に関わっているのかを容易に理解できる。
- Viewを大幅に変えることになっても、基本的にModelやController、Routerは変更しなくて済む（変更に関わる労力も最小限で済む）

## 名前空間を使う理由

- Webアプリ開発は他人の作ったライブラリを導入しながら開発を進めることになる（Laravelプロジェクト自体も他人が作ったライブラリ）
- クラスの重複を防ぐため（名前空間によって「どのクラス名を指しているのか」を明示する必要がある）
- 万一クラス名が重複してしまうと、コンピュータがどちらのクラスを読み込んでいいか分からなくなってしまい、処理が正常に機能しない可能性が高くなる

## ファサード

- クラスを使いやすくしたもの
- サービスを利用する入り口となるclassを提供する
- ファサードを使えばただ用意されているクラスからメソッドを呼び出すだけでOK（インスタンスの取得が不要）

※通常のサービスは「インスタンスの取得」→「その中からメソッドを呼び出す」で利用する

## 例：Authファサード

```php
Auth::check()：ユーザがログイン状態にあるかどうかを判定する
Auth::user()：ログイン中のユーザを取得する
```

## ファサードの設定

下記ファイルの aliases の中で設定をされている。

laravel-quest＞config＞app.php

## 1対多

- 動画 （Movie）は、利用者（User）に所属（belongsTo）している
- 利用者（User）は、動画（Movie）を所有（hasMany）している

## マイグレーション

### unsigned()

- マイナスの値は保存できないように制限を掛けているということ

### index

- 検索速度を早めることができる関数
- カラムに付けることで、 index() が付けられたカラムのみを抽出して素早く対象動画の情報を得ることができる

## 外部キー制約

    $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');

このコードの意味合いは、

- このテーブルのカラム`('user_id')`と、usersテーブルのカラム`('id')`が一致していて、別テーブルはusersテーブルのことを示す
- idカラムを持っているレコード（ユーザ自体）が削除された場合は、この動画情報も一緒に削除される

## モデルのリレーション

モデルの作成

    $ php artisan make:model Movie

- 下記の様にモデルの中身に、`fillable`変数を定義することで下記３つのカラム`（'user_id','url','comment'）`を一度に入力→保存できるようになる
- ここでモデルに１対多の関係を定義するために、`user()`という関数を定義
- 関数の中身としては、`return $this->belongsTo(User::class);`という値を返す
- この内容は「MovieモデルがUserモデルに所属している」ということを明示する役割があり、実際のコード記述上でも、下記のようなシンプルなコードで「Movieインスタンスが属しているユーザを取得」できることになります。

```php
$movie->user()->get();
$movie->user;
```

Userモデル にも１対多の関係を定義しましょう。

- Movieモデルの`user()`は単数形で表現していたと思うのですが、Userインスタンスが所有するMovieは複数あるので、`movies()`という複数形にする（1人が複数のmovieを投稿可能なので）

関数の中身としては、return $this->hasMany(Movie::class); という値を書き込みます。
この内容は「 User モデルがMovieモデルを所有している」ということを明示する役割があり、
実際のコード記述上でも、下記のようなコードで「Userインスタンスが所有しているMovieを取得」できることになります。

```php
$user->movies()->get();
$user->movies;
```

## resource

```php
Route::resource('movies', 'MoviesController');
```

laravel-quest/routes/web.php

web.php(一部抜粋)

```php
Route::get('/', 'UsersController@index'); //書き換え

Route::get('signup', 'Auth\RegisterController@showRegistrationForm')->name('signup');
Route::post('signup', 'Auth\RegisterController@register')->name('signup.post');

Route::get('login', 'Auth\LoginController@showLoginForm')->name('login');
Route::post('login', 'Auth\LoginController@login')->name('login.post');
Route::get('logout', 'Auth\LoginController@logout')->name('logout');

// 追記分
Route::resource('users', 'UsersController', ['only' => ['show']]);

Route::group(['middleware' => 'auth'], function () {
    Route::resource('movies', 'MoviesController', ['only' => ['create', 'store', 'destroy']]);
});
```

最後に記述しているMoviesControllerに注目して下さい。
ここでは、Route::groupでルーティングのグループを作成して、その時に`['middleware' => 'auth']`として、ログイン認証を通ったユーザのみが、その内部のルーティングにアクセスできるようにしています。

また、Route::resource()で、７つのルーティングの短縮形となるのですが
あえて`['only' => ['create', 'store', 'destroy']]`と記述して、実際にルートとして設定するアクションを限定しています。

# APIキーと環境変数

アプリケーション上でAPIキーを使う場合、原則的にAPIキーを「環境変数」として設定します。

APIキーは外部に流出すると悪用される危険があるので、他人に見せることはしてはいけないのですが
APIキーを記載したコードをそのままリモートリポジトリ等にプッシュすると
コードと一緒にAPIキーまでもが、全世界に公開されてしまい危険な状態になります。

なので、漏洩の危険がないように、APIキーを環境変数として指定してやるということを行います。

では、アプリケーションの .env ファイルのどの場所でも良いので、
先ほど取得したAPIキーを記載していきましょう。

laravel-quest/.env

```php
API_KEY={あなたのAPIキー}
```
Laravelプロジェクトにおいて、.envファイルに指定したAPIキーはそのまま利用される訳ではなく、通常キャッシュを用いて利用されます。

Laravelではアプリの起動時に複数のファイルを読み込んでいて、その実行速度を早めるために
起動のたびに.envファイルの環境変数を読み込むのではなく、キャッシュを利用して環境変数を読み込んでいるからです。

なので、configフォルダの中のファイルの`app.php`に、環境変数を読み込ませるための記述をしていきましょう。

laravel-quest/config/app.php

```php

    //(前略)

    'aliases' => [

        'App' => Illuminate\Support\Facades\App::class,
        'Artisan' => Illuminate\Support\Facades\Artisan::class,
        'Auth' => Illuminate\Support\Facades\Auth::class,
        'Blade' => Illuminate\Support\Facades\Blade::class,
        'Broadcast' => Illuminate\Support\Facades\Broadcast::class,
        'Bus' => Illuminate\Support\Facades\Bus::class,
        'Cache' => Illuminate\Support\Facades\Cache::class,
        'Config' => Illuminate\Support\Facades\Config::class,
        'Cookie' => Illuminate\Support\Facades\Cookie::class,
        'Crypt' => Illuminate\Support\Facades\Crypt::class,
        'DB' => Illuminate\Support\Facades\DB::class,
        'Eloquent' => Illuminate\Database\Eloquent\Model::class,
        'Event' => Illuminate\Support\Facades\Event::class,
        'File' => Illuminate\Support\Facades\File::class,
        'Gate' => Illuminate\Support\Facades\Gate::class,
        'Hash' => Illuminate\Support\Facades\Hash::class,
        'Lang' => Illuminate\Support\Facades\Lang::class,
        'Log' => Illuminate\Support\Facades\Log::class,
        'Mail' => Illuminate\Support\Facades\Mail::class,
        'Notification' => Illuminate\Support\Facades\Notification::class,
        'Password' => Illuminate\Support\Facades\Password::class,
        'Queue' => Illuminate\Support\Facades\Queue::class,
        'Redirect' => Illuminate\Support\Facades\Redirect::class,
        'Redis' => Illuminate\Support\Facades\Redis::class,
        'Request' => Illuminate\Support\Facades\Request::class,
        'Response' => Illuminate\Support\Facades\Response::class,
        'Route' => Illuminate\Support\Facades\Route::class,
        'Schema' => Illuminate\Support\Facades\Schema::class,
        'Session' => Illuminate\Support\Facades\Session::class,
        'Storage' => Illuminate\Support\Facades\Storage::class,
        'URL' => Illuminate\Support\Facades\URL::class,
        'Validator' => Illuminate\Support\Facades\Validator::class,
        'View' => Illuminate\Support\Facades\View::class,

    ],
    
    'key_name' => env('API_KEY'),//追記

];

```

# RESTful API
YouTube API では、URLとHTTPメソッド（GETなど）を用いて動画のタイトルなどを取得してきました。

HTTPメソッドを使って決まったルールでアプリケーションにアクセスできるようにする考え方をREST（REpresentational State Transfer）といい、
その考え方に基づいて作られた YouTube API のような外部アプリケーションとの接続のルールを
RESTful APIといいます。

少し発展的な内容にはなりますが、学んでいただくことで
Webサービスでは現状主流である「自社アプリケーションを他社サービスに連携させる」という
エンジニア業務上大切な仕事についてもある程度理解できると思います。

まず、初めにRouterから作っていきましょう。
前提として、URLの末尾に「/rest」を付けると、RESTful APIを扱えるようにしていきます。