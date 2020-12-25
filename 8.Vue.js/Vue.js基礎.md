# Vue.js基礎

- 通常のJavaScriptやjQueryを使う場合、DOMを直接操作するコード（要素の書き換えやイベントに関する処理）をたくさん書くことになり、コードが乱雑になりがちです。しかしVue.jsを使うことで、よりシンプルにDOM関係の処理を記述できます。
- 1つのプログラムにVue.jsとjQueryは共存可能
- DOM要素の指定やAjaxでのデータ取得など、jQueryの命令を使ったほうが便利な場合もある

## 基本形
js
```js
const app = new Vue({
  el: '#example',
  data: {
    greeting: 'Hello Vue.js!',
  },
});
```
- Vueインスタンス（`new Vuew〜`）を他で使わない場合は変数`app`に格納しなくてもOK

html
```html
<div id="example">{{ greeting }}</div>
```

- `new Vue`：Vueオブジェクトのインスタンス
- `new Vue`の引数はオブジェクト（オプションオブジェクトと言う）
- `el`、`data`：プロパティ
- DOM操作が自動で行われる
- データバインディングをするとデータの変更がHTMLに自動で反映される

## ディレクティブ（DOM操作を自動化できる）
- `v:bind`・・・データバインディング（`:`に省略可能）
- `v-for`：繰り返し処理
- `v-on`：イベント処理（`@`に省略可能）
- `v-model`：v:bind+v:onの双方向データバインディング
- `v-show`：表示・非表示切り替え
- `v-if`：条件分岐
- `v-once`：初回だけデータバインディングをしてそれ以降は静的コンテンツとして扱う
- `v-pre`：要素と全ての子要素のコンパイルをスキップ
- `v-html`：HTMLとして使える

https://jp.vuejs.org/v2/api/#%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96

## 双方向データバインディング
### v:bind+v:on
普通の書き方
```html
<div id="example">
  <input
    v-bind:value="name"
    v-on:input="name = $event.target.value"
  >
  <p>{{ name }}</p>
</div>
```
省略形
```html
<div id="example">
  <input
    :value="name"
    ＠input="name = $event.target.value"
  >
  <p>{{ name }}</p>
</div>
```

```js
const app = new Vue({
  el: '#example',
  data: {
    name: '太郎',
  },
});
```

### v-model
```js
<div id="example">
  <input v-model="name">
  <p>{{ name }}</p>
</div>
```

- v-bind：データ→テンプレートのデータバインディング
- v-on：テンプレート→データのデータバインディング
- v-model：v:bindとv-onをどちらもやってくれる

## Vue.js内で処理を書く場合
### 算出プロパティ
html 
```html
<div id="example">
  <div>
    <p>身長： <input v-model="height" type="text"> cm</p>
    <p>体重： <input v-model="weight" type="text"> kg</p>
  </div>
  <div>
    BMI: {{ bmi }}
  </div>
</div>
```
js
```js
const app = new Vue({
  el: '#example',
  data: {
    height: '',
    weight: '',
  },
  computed: {
    bmi() {
      if (this.height && this.weight) {
        // センチメートルをメートルに変換する
        const meterHeight = this.height / 100;
        // BMIを計算する
        const bmi = this.weight / (meterHeight * meterHeight);
        // 小数点以下の桁数を、２桁にして返す
        return bmi.toFixed(2);
      }

      return '';
    },
  },
});
```

### メソッド
html 
```html
<div id="example">
  <div>
    <p>身長： <input v-model="height" type="text"> cm</p>
    <p>体重： <input v-model="weight" type="text"> kg</p>
  </div>
  <div>
    BMI: {{ getBmi() }}
  </div>
</div>
```
js
```js
const app = new Vue({
  el: '#example',
  data: {
    height: '',
    weight: '',
  },
  methods: {
    getBmi() {
      if (this.height && this.weight) {
        // センチメートルをメートルに変換する
        const meterHeight = this.height / 100;
        // BMIを計算する
        const bmi = this.weight / (meterHeight * meterHeight);
        // 小数点以下の桁数を、２桁にして返す
        return bmi.toFixed(2);
      }

      return '';
    },
  },
});
```
### 算出プロパティとメソッドの使い分け
大きな違いは`算出プロパティはキャッシュされる`こと

- 算出プロパティは参照しているVueインスタンスのデータが更新されたときだけ再計算
- メソッドは再描画が起きるたびに実行される
- 基本的に算出プロパティを使うことで余分な処理の実行を防ぐ
- メソッドは呼ばれるために実行する必要のある処理で使う

## 記事
- [vue.js の {{name}} とかが html に表示されるのを防ぐ(v-cloak)](https://qiita.com/macoshita/items/630e6bf1b6fa068790a3)
- [他のフレームワークとの比較](https://jp.vuejs.org/v2/guide/comparison.html)