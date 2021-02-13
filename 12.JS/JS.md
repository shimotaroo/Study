## 【jQuery】
- closet()：直近の祖先の要素を取得。(‘div’)とかにすると直近のdiv要素を取得
- select[name*=schoolCourse]：name属性に「schoolCourse」を含むselect要素
- $('.c1’, ‘#d2’)：id=‘d2'の要素の中にあるclass=‘c1’の要素を取得
- prop(‘checked’)：セレクターにcheckedプロパティがあるかチェック（あればtrueを返す）
- nextAll()：その要素より後段の兄弟要素（同じ階層）
- focus()：特定の要素にフォーカスを当てる。入力フォームに使うとページ訪問時に初めから選択されている状態にできる。ユーザーの手間を省く
- セレクターで属性値に変数を使う場合
```js
$("[name*=mProductRentalId][value="+ rentalOptionSelectId +"]", optionsArea).each(function () {
});
```
