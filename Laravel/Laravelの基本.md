# MVC
Laravelを学ぶ上で一番重要と言っても過言ではない、MVC(+Router)について解説していきます。下記の図をご覧下さい。

前提として、Laravelはサーバ上で動きます。

ユーザがブラウザ（BROWSER）を開いてウェブアプリケーションを閲覧したい時、
ユーザから「アプリ表示をお願いします！」と指令を出す行為がHTTPリクエスト、
サーバ側が「アプリ表示はコレです、どうぞ！」とページ表示内容を返す行為がHTTPレスポンスです。

上記図の一番上：DATABASEと一番下：BROWSERを除いた、下記４つの要素に注目して下さい。これらの要素が、サーバ側：Laravel上で動きます。

Router（ルータ）
Mocel（モデル）
Controller(コントローラ)
View（ビュー）
それぞれ解説していきます。

Routerは、ブラウザから来たリクエストを「どの処理（Controllerなど）へ送るか」を判断する交通整理的な役割を果たします。
このRouterの機能がなければ、いくら素晴らしいアプリケーションを作ったとしても、アプリケーションの処理自体が始まりません。

Modelとは、MVCにおける「データベース」関連の処理を担当する部分です。例えば、アプリケーションに新規登録するユーザの情報を保存する際に、このモデルを通じて情報がデータベースに保存されます。

Controllerは、Laravelにおける実行係です。Controllerがサーバ上のアプリケーションの機能実行を担います。
Routerに指定されたControllerが特定の処理を行い、Modelを操作して、新規ユーザ作成や保存処理などを行うのです。

Viewは表示機能を担います。端的にいうとViewはWebページそのものと捉えていただいて差支えありません。
Controllerが処理した結果をもとに、Webページを出力して最終的にユーザに見せる役割です。Viewがないとユーザは何も情報を取得できないことになります。

４つの要素を見てきましたが、なぜこれらの役割を分けるのか疑問に思う方もいるかもしれません。

これら４つの要素は、全てコードで書かれています。
全てを１つのファイルにまとめることも可能なのですが、それぞれの役割を分離することで、誰が見ても分かりやすいコードとなるのです。

仮に細かい命令が増えてしまったとしても、これらModel/Router/Controller/Viewの役割さえ理解していれば、どのコードがどの命令に関わっているのかを容易に理解することができます。
また、これらの役割分けは変更にも強く、例えば見た目に関わるViewを大幅に変えることになっても、基本的にModelやController、Routerは変更しなくて済むので、変更に関わる労力も最小限で済むというメリットがあるのです。

今回の講義ではMVC(+Router)についての紹介はここまでですが、次回の講義以降、MVCを中心に話を進めていきますので、「ここで学んだMVC(+Router)は重要な内容である」ということは覚えておいて下さい。

# 名前空間
なぜ、名前空間というものを使うのでしょうか。
このLaravelプロジェクト自体もそうですが、これから他人の作ったライブラリを導入しながら開発を進めることになります。


その場合に、万一クラス名が重複してしまうと、コンピュータがどちらのクラスを読み込んでいいか分からなくなってしまい、処理が正常に機能しない可能性が高くなります。
このような「クラス名の重複」を防ぐためにも、名前空間によって「どのクラス名を指しているのか」を明示する必要があるのです。

「どのクラスを指しているか」を誰が見ても分かりやすく明示するために、
原則として上記のように「コントローラファイルのフォルダ構成」と、名前空間を一致させて使うようにしましょう。

# ファサード
ファサードとは？
Auth::check()を使いましたが、そもそもAuthとは何でしょうか？
これはファサードと言われるものです。

ファサードは、クラスを使いやすくしたものと思っていただければいいでしょう。
例えば、通常下記のように長々と表現しないといけないクラスを

※サンプルコードにつき追記不要です

Illuminate\Support\Facades\Auth::class
以下のように、短縮して呼び出しやすくしているのです。

Auth
改めて、Authファサードを利用した関数には、下記のようなものがあります。

Auth::check()：ユーザがログイン状態にあるかどうかを判定する
Auth::user()：ログイン中のユーザを取得する
ちなみに、ファサードは、 下記ファイルの aliases の中で設定をされています。

laravel-quest＞config＞app.php

# 1対多

- 動画 （Movie）は、利用者（User）に所属（belongsTo）している
- 利用者（User）は、動画（Movie）を所有（hasMany）している

# マイグレーション
また、user_idについている unsigned() , index()とは何でしょうか。

unsigned() は、マイナスの値は保存できないように制限を掛けているということです。
index() は、検索速度を早めることができる関数です。カラムに付けることで、 index() が付けられたカラムのみを抽出して素早く対象動画の情報を得ることができます。
動画は特にユーザとの関わりが深いため（どのユーザが所有しているかが重要であるので）、user_idにindex()を付けています。

# 外部キー制約

外部キー制約とは？
xxxxx_ create_movies_table.php(一部抜粋)

マイグレーションファイルのうち、下記コードは「外部キー制約」といわれるコードです。

$table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
このコードの意味合いは、
「このテーブルのカラム('user_id')と、usersテーブルのカラム('id')が一致していて、別テーブルはusersテーブルのことを示す。また、idカラムを持っているレコード（ユーザ自体）が削除された場合は、この動画情報も一緒に削除される」
ということです。

# モデルのリレーション
次は以下のコマンドにて、Movie モデルを作成していきます。

ターミナル

$ php artisan make:model Movie
まず、下記の様にモデルの中身に、fillable変数を定義することで
下記３つのカラム（'user_id','url','comment'）を一度に入力→保存できるようにしましょう。

また、ここでモデルに１対多の関係を定義するために、user() という関数を定義します。
関数の中身としては、return $this->belongsTo(User::class);という値を書き込みます。
この内容は「MovieモデルがUserモデルに所属している」ということを明示する役割があり、
実際のコード記述上でも、下記のようなシンプルなコードで「Movieインスタンスが属しているユーザを取得」できることになります。

$movie->user()->get();
$movie->user;

続いて、Userモデル にも１対多の関係を定義しましょう。
Movieモデルの user() は単数形で表現していたと思うのですが、Userインスタンスが所有するMovieは複数あるので、movies() という複数形を使って関数を作りましょう。
関数の中身としては、return $this->hasMany(Movie::class); という値を書き込みます。
この内容は「 User モデルがMovieモデルを所有している」ということを明示する役割があり、
実際のコード記述上でも、下記のようなコードで「Userインスタンスが所有しているMovieを取得」できることになります。

$user->movies()->get();
$user->movies;

# resource
実は、前回のテキストで書いていたような「７つのルーティング」記述を省略する方法があります。下記の通りです。

※サンプルコードにつき、まだ記述不要です。

Route::resource('movies', 'MoviesController');
前回までは１つ１つのルーティングを記述してきましたが、上記は１つの記述で７つのルーティングを準備できる、ルーティング記述の短縮バージョンです。

今回から、この短縮バージョンも利用して記述していきましょう。
ルーティングを以下の様に書き換えます。

laravel-quest　＞　routes　＞　web.php

web.php(一部抜粋)

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
最後に記述しているMoviesControllerに注目して下さい。
ここでは、Route::groupでルーティングのグループを作成して、その時に['middleware' => 'auth']として、ログイン認証を通ったユーザのみが、その内部のルーティングにアクセスできるようにしています。

また、Route::resource()で、７つのルーティングの短縮形となるのですが
あえて['only' => ['create', 'store', 'destroy']]と記述して、実際にルートとして設定するアクションを限定しています。

# APIキーと環境変数
アプリケーション上でAPIキーを使う場合、原則的にAPIキーを「環境変数」として設定します。

APIキーは外部に流出すると悪用される危険があるので、他人に見せることはしてはいけないのですが
APIキーを記載したコードをそのままリモートリポジトリ等にプッシュすると
コードと一緒にAPIキーまでもが、全世界に公開されてしまい危険な状態になります。

なので、漏洩の危険がないように、APIキーを環境変数として指定してやるということを行います。

では、アプリケーションの .env ファイルのどの場所でも良いので、
先ほど取得したAPIキーを記載していきましょう。

laravel-quest　＞　.env

.env

API_KEY={あなたのAPIキー}
Laravelプロジェクトにおいて、.envファイルに指定したAPIキーはそのまま利用される訳ではなく、通常キャッシュを用いて利用されます。

Laravelではアプリの起動時に複数のファイルを読み込んでいて、その実行速度を早めるために
起動のたびに.envファイルの環境変数を読み込むのではなく、キャッシュを利用して環境変数を読み込んでいるからです。

なので、configフォルダの中のファイルの app.php に、環境変数を読み込ませるための記述をしていきましょう。

laravel-quest　＞　config　＞　app.php

app.php

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