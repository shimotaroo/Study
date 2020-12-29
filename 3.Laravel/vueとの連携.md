# 大まかな手順

1. Vueをインストール
2. コンポーネント作成
3. app.jsの編集（resources/js/app.jsは、Laravelの全画面で共通的に使用することを想定したJavaScript）
```js
import './bootstrap'
import Vue from 'vue'
import ArticleLike from './components/ArticleLike'

const app = new Vue({
  el: '#app',
  components: {
    ArticleLike,
  }
})
```
4. webpack.mix.js（Laravel Mixの設定ファイル）の編集
5. トランスパイル（`npm run watch-poll`）
6. トランスパイルしたJavaScripuをbladeに読み込ませる
```php
<!DOCTYPE html>
<html lang="ja">
<head>
  {{--略--}}
</head>

<body>
  <div id="app"> {{--この行を追加--}}
    @yield('content')
  </div> {{--この行を追加--}}

  <script src="{{ mix('js/app.js') }}"></script> {{--この行を追加--}}
  <!-- JQuery -->
  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>  
  {{--略--}}
</body>
```
7. BladeにVueコンポーネントを読み込ませる
```php
<div class="card-body pt-0 pb-2 pl-3">
    <div class="card-text">
      <article-like>
      </article-like>
    </div>
  </div>
```

# Vueを使うときの.gitignore
以下を追記
```
/public/css
/public/js  
/public/mix-manifest.json
```
