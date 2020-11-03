# 公式ドキュメント

array_map — 指定した配列の要素にコールバック関数を適用する

説明
```php
array_map ( callable $callback , array $array1 [, array $... ] ) : array
```

array_map() は

- array1 (および、 それ以上の配列が与えられた場合は ...) の各要素に callback を適用した後
- 適用後の要素を含む array を返します。 
- callback 関数が受け付けるパラメータの数は、 array_map() に渡される配列の数に一致している必要があります。

# 例文

```php
$data['editDetail']['reservationSchoolDetail'] = array_map(function ($detail) {
    $detail['reservationStudentDetail'] = $detail['schoolMember'];
    return $detail;
}, $data['details']);
```

処理の内容

`function($detail)`の仮引数`$detail`に`array_map`の第二引数の`$data['details']`を格納して処理する
処理内容は

```php
    $detail['reservationStudentDetail'] = $detail['schoolMember'];
    return $detail;
```

なので

```php
    $data['details']['reservationStudentDetail'] = $data['details']['schoolMember'];
    return $data['details'];
```

つまり、`$data['details']['reservationStudentDetail']`に`$data['details']['schoolMember']`を格納してそれを返り値にする。
それを`$data['editDetail']['reservationSchoolDetail']`に格納している。

最終的には
**$data['editDetail']['reservationSchoolDetail'] = $data['details']['schoolMember']**になるってこと。
