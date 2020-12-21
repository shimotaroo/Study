# try-catchとトランザクション

## try-catch
- スクリプトを実行したときにデータベースが存在しない
- 読み込もうとしたファイルが存在しない
このようなエラー（例外）が発生しても正常にスクリプトが動作するようにすることを`例外処理`といいます。

```php
try {
  // 例外が発生するおそれがあるコード
} catch(例外クラス名　例外を受け取る変数名){
  // 例外発生時の処理
}
```

サンプルコード
```php
try{
  echo warizan(6,0);
}catch(Exception $e){
  echo "エラー：" . $e->getMessage();
}

// 例外の判定基準は、warizan 関数に記述
// 例外の判断
function warizan($num1,$num2){
  if( $num2 == 0 ){
  throw new Exception("０では除算できません");
  }
return $num1 / $num2;
}
```

- 例外が発生しそうな処理（DBとのやりとり、ファイルアップロード系が多い）をする大枠に例外処理を設定（Controllerに書くのが良さそう）
- tryの中で呼び出しているメソッドの中で例外判定を書いて`throw new Exception`を書く
- `throw new Exception`に処理が流れたら`catch`の方に処理が流れてエラーメッセージを表示させる
- これがないとデバッグしないモードだと画面が真っ白になる（と思うので）クライアントにエラーを知らせるために必要

## 参考
https://laraweb.net/surrounding/2192/
## トランザクション
- トランザクションはControllerに書く（Modelに書かない）
- 複数の処理をまとめたもの（ただし、これらの複数の処理は分離できない）
- トランザクションは成功か失敗のいずれかしかない
- 中途半端に成功とか失敗とかない。失敗したらトランザクション実行前に戻る
- トランザクションの中の一連の処理が全て成功したらコミット→処理内容をDBに反映
- トランザクションの中のどこかが失敗したらロールバック→トランザクション実行前のDBの状態に戻す

### Laravelでのトランザクション
#### DBファサードのtransactionメソッドを使用

```php 
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});

DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
}, 5);
```
- この中で例外が投げられたら自動でコミットとロールバックを行う
- `transaction`メソッドは第２引数に、デッドロック発生時のトランザクション再試行回数を指定できます。試行回数を過ぎたら、例外が投げられます。

#### 手動トランザクション
```php
DB::beginTransaction();

DB::rollBack();

DB::commit();
```

## 参考
https://qiita.com/zd6ir7/items/6568b6c3efc5d6a13865