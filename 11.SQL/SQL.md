■GROUP_CONCAT関数
```
GROUP_CONCAT(toi.product_name SEPARATOR "\t”)
```

■SEPARATOR
取得するデータに使われない区切り文字を指定する（\tはタブ）
ここにカンマを指定したとして、データにカンマが使われている場合、implodeした時にデータがカンマのところで分裂してしまう

■重複したデータは取得しない方法（DISTINCT）
厳密にいうと１レコードにまとめる
https://www.sejuku.net/blog/54990
https://www.dbonline.jp/mysql/select/index13.html

■テーブル結合
https://qiita.com/zaburo/items/548b3c40fee68cd1e3b7

■テーブルにカラム追加
https://sql55.com/t-sql/t-sql-alter-table-1.php

■インデックスとは
https://blog.takanabe.tokyo/2014/11/mysql%E3%81%AE%E3%82%A4%E3%83%B3%E3%83%87%E3%83%83%E3%82%AF%E3%82%B9%E3%81%AE%E5%BD%B9%E5%89%B2/

■外部キー制約
https://qiita.com/SLEAZOIDS/items/d6fb9c2d131c3fdd1387

■EXISTS関数の使い方
https://style.potepan.com/articles/19151.html

・１番左のIDは自動採番（Auto Incremence）の設定をしているのでINSERT文には記載しない
（カラム名、VALUEどちらも）
・現在年月日、現在時刻をカラムがある場合はCURRENT_DATE()、CURRENT_TIME()を書く
・リレーション系のDB（どこかのテーブルとどこかのテーブルからIDを持ってきて紐付ける）の場合は、
```
SET @変数名 = (SELECT カラム名 FROM テーブル名 WHERE 条件式)
```
でIDを変数に入れてそれを使う。（IDは固定値を書かない）
理由→1つのPJで開発者ごとにDBを持つ場合、そのテーブルではたまたまそのIDになっただけで、
本番環境にリリースするときに本番用のDBのそのIDに既に別のデータが入ってしまっている場合に、
INSERTエラーになってしまうから。
どんな環境（開発、本番）でもSQL文が実行できるように可変する可能性があるものは変数で表現する
・同じテーブルにINSERTする時は
VALUES (挿入する値 ), (挿入する値);
この形にする

■CASE
https://www.sejuku.net/blog/73639

■CONCATとGROUP_CONCAT
https://nodoame.net/archives/6095

■IS NULL、IS NOT NULL
https://www.sejuku.net/blog/72480

■特定の条件だけWHEREの条件を追加
```php
$today = date('Y-m-d'); 
            $QueryBuilder = $CouponModel->getRepository()->createQueryBuilder('c');
            $QueryBuilder->where(
                'c.SkiResort = :SkiResort AND c.statusType = :statusType AND c.deleteFlag = :deleteFlag
                AND true = CASE
                    WHEN (c.reservationFromDate IS NOT NULL AND c.reservationToDate IS NOT NULL) AND c.reservationFromDate <= :today AND c.reservationToDate >= :today THEN true
                    WHEN (c.reservationFromDate IS NULL AND c.reservationToDate IS NULL) THEN true
                    ELSE false
                END'
            );
            $QueryBuilder->setParameters([
                'SkiResort' => $SkiResort,
                'statusType' => CouponModel::getPropertyValue('statusType', 'COUPON_STATUSTYPE_ENABLE'),
                'deleteFlag' => 0,
                'today' => $today,
            ]);
            $QueryBuilder->orderBy('c.couponId', 'ASC');
            
            $this->CouponObjects = $QueryBuilder->getQuery()->getResult();
```
