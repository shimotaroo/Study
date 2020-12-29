# 開発ポイント（メモ）
-  初期でファサード登録されてるnamespaceは略称で使える（ex: use DB;）
- モデルは`/app/Model/`配下に作成（Modelディレクトリ作成）
ただしUser.phpはそのまま移動させない方が良い→Authとかでデフォルトのnamespaceを使用しているから
- メソッドは`PHPDoc`で書く
- DBとのやりとりはModelに書く
- Controllerはなるべく処理を追うだけにする（なぜかLaravelはControllerに何でも盛り込むのを推奨！？）
- バリデーションもControllerに書かない方がベター
- ルーティングの優先度に注意する（[Laravelのルーティング優先度でつまづいた話](https://qiita.com/dhiki1234/items/dcb2d2bc17d11e1f4565)）
- クラス内でしか使わないメソッドは`private`にして、`_メソッド名`にする（これはPHP？Laravel？のどっちの決まり？）
```php
private function _getCheckboxData()
{

}
```
- middlewareの付与の仕方
    1. Controllerのコンストラクタで書く
    2. web.phpで`->middleware('auth')`をつける
- 外部キー制約は参照元のテーブルの`id`カラムしか使えない？
- DBへの登録・更新処理の場合は`try/catch(例外処理)`と`トランザクション`を使う
- 関数の引数が多い場合は配列に入れて関数に渡してあげる（4つ以上が目安）

# browserSyncのエラー

```npm ERR! code 1
npm ERR! path /var/www
npm ERR! command failed
npm ERR! command sh -c cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --config=node_modules/laravel-mix/setup/webpack.config.js "--watch" "--watch-poll"

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2020-12-27T01_32_37_482Z-debug.log
npm ERR! code 1
npm ERR! path /var/www
npm ERR! command failed
npm ERR! command sh -c npm run development -- --watch "--watch-poll"

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2020-12-27T01_32_37_513Z-debug.log
npm ERR! code 1
npm ERR! path /var/www
npm ERR! command failed
npm ERR! command sh -c npm run watch -- --watch-poll

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2020-12-27T01_32_37_545Z-debug.log
```
- 原因
    - そもそも`browserSync`がインストールされてない（`package-lock.json`に記載がない）

- 解決方法
    - 以下の実行
```
docker-compose exec workspace npm install browser-sync-webpack-plugin --save-dev
```
vuespa/webpack.mix.js
```js
const mix = require('laravel-mix');

/*
 |--------------------------------------------------------------------------
 | Mix Asset Management
 |--------------------------------------------------------------------------
 |
 | Mix provides a clean, fluent API for defining some Webpack build steps
 | for your Laravel application. By default, we are compiling the Sass
 | file for the application as well as bundling up all the JS files.
 |
 */

mix.browserSync({
    proxy: 'nginx',
    files: [
        './resources/**/*',
        './public/**/*',
    ],
    open: false,
    reloadOnRestart: true,
})
    .js('resources/js/app.js', 'public/js')
    .version();

```