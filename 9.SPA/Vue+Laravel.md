# 手順

## Vue-Routerインストール

    npm install --save-dev vue-router

## ルーティング設定
- PJディレクトリ/resources/js/router.jsを作成<br>
URLと使うVue.jsのコンポーネントの対応関係を記載

- PJディレクトリ/resources/js/app.jsを作成<br>

## Axiosのインストール
- AxiosとはHTTP通信を簡単に行うことができるJavascriptライブラリ

    npm install --save-dev axios

##　ルーティング
- 画面返却：routes/web.php
- API：routes/api.php
- app/Providers/RouteServiceProvider.phpを修正する
```php
protected function mapApiRoutes()
{
    Route::prefix('api')
         ->middleware('web') // ★ 'api' → 'web' に変更
         ->namespace($this->namespace)
         ->group(base_path('routes/api.php'));
}
```

## テスト駆動が好ましい？

>実装してすぐにプログラムの動作を確認できるように、テストコードを書きながら開発しましょう。テストコードがないとフロントエンドを実装して API を呼び出すコードを作り終わるまでプログラムの正しさが分かりませんが、それでは不便

テスト方法
- テストコードを書く（phpUnit）
- Postmanを使う　https://www.postman.com/

## VueRouter

- RouterLink で描画したリンクをクリックすると、フロントエンドでの画面遷移、つまり Vue Router によるコンポーネントの切り替わりが発生
- サーバサイドへの通信回数を最適化して SPA の利点を活かすため、Vue Router の管理下にあるページへ遷移する場合は必ず RouterLink を使用
- Vue.js入門 (3) クラスとスタイル https://www.hypertextcandy.com/vuejs-introduction-class-style/

## @submit
- @submit に続く .prevent はイベント修飾子と呼ばれます。.prevent を記述することは、イベントハンドラで event.preventDefault() を呼び出すのと同じ効果があります。これによりデフォルトのフォーム送信の挙動をキャンセルしページリロードを抑制

## Vuex
- 複数のコンポーネントを跨ぐデータのやりとりを行う場合に使う
- https://vuex.vuejs.org/ja/
- https://qiita.com/kahirokunn/items/f764302db290a504cc19
- https://qiita.com/knhr__/items/5fec7571dab80e2dcd92
- ReactでいうRedux

## バッククオートの使い方

```js
const response = await axios.get(`/api/photos/?page=${this.$route.query.page}`)
```