# CSVインポート
## 方法
- `\SplFileObject`クラスを使う
- ライブラリ(`(goody/csv)`)を使う
## サンプルコード（`\SplFileObject`クラスを使う）
```php
```
## 参考情報
### PHP文字コード関連
- 文字コード変換：[mb_convert_encoding](https://www.php.net/manual/ja/function.mb-convert-encoding.php)
- 文字コード取得：[mb_detect_encoding](https://www.php.net/manual/ja/function.mb-detect-encoding.php)
### `\SplFileObject`クラスを使う
- [Laravelで大容量CSVファイルをSplFileObjectクラスで確実に処理する](https://www.ritolab.com/entry/63)
- [LaravelでCSVファイルのインポート・エクスポートをやってみた](https://tech.arms-soft.co.jp/entry/2019/10/23/090000)

### ライブラリ(`(goody/csv)`)を使う
- [CSVインポート機能の作成](https://laraweb.net/tutorial/5906/)