# constとdefineをの違い（定数）

## 大きな違い
- defineは関数、constは関数ではない→constの方が処理は早い
```php
<?php
define('HOGE', 'hoge');
const FUGA = 'fuga';
```

## 参考情報
- [PHPで定数定義するときはdefine()よりもconstを使うべき](https://qiita.com/Hiraku/items/bb0cb665d830f7cd37ff)
- [PHPの「define」と「const」の違い](https://qiita.com/schrosis/items/485b984e05b2eb4521b4)
- [(php) constとdefineの違い](http://yukiriru.hatenablog.com/entry/2015/08/07/102328)

