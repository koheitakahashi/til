# TEXT 型か VARCHAR 型か

結論、パフォーマンスの観点から VARCHAR の方が好ましい。
TEXT 型はインデックスの作成に制限があり、VARCHAR の方が柔軟にインデックスを作成できるため、パフォーマンスに差が出る。

補足だが、TEXT 型と VARCHAR ともに最大文字列長は 65,535 文字である。

# 参考
- [VARCHAR vs\. TEXT for MySQL Databases \| cPanel Blog](https://blog.cpanel.com/varchar-vs-text-for-mysql-databases/)
- [MySQL :: MySQL 8\.0 リファレンスマニュアル :: 11\.3\.4 BLOB 型と TEXT 型](https://dev.mysql.com/doc/refman/8.0/ja/blob.html)
- [MySQL :: MySQL 8\.0 リファレンスマニュアル :: 11\.3\.2 CHAR および VARCHAR 型](https://dev.mysql.com/doc/refman/8.0/ja/char.html)
