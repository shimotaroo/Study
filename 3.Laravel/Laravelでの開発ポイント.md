-  初期でファサード登録されてるnamespaceは略称で使える（ex: use DB;）
- モデルは`/app/Model/`配下に作成（Modelディレクトリ作成）
ただしUser.phpはそのまま移動させない方が良い→Authとかでデフォルトのnamespaceを使用しているから
- メソッドは`PHPDoc`で書く
- DBとのやりとりはModelに書く
- Controllerはなるべく処理を追うだけにする（なぜかLaravelはControllerに何でも盛り込むのを推奨！？）
- バリデーションもControllerに書かない方がベター
- ルーティングの優先度に注意する（[Laravelのルーティング優先度でつまづいた話](https://qiita.com/dhiki1234/items/dcb2d2bc17d11e1f4565)）
- クラス内でしか使わないメソッドは`private`にして、`_メソッド名`にする（これはPHP？Laravel？のどっちの決まり？）
- middlewareの付与の仕方
    1. Controllerのコンストラクタで書く
    2. web.phpで`->middleware('auth')`をつける
