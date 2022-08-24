# ActiveRecord
## abstract_class の使い時

STI を使っているとき、その抽象クラスで abstract_class = true にすると table と model が1対1対応しないことを明示できる。

### 参考

- [abstract_class のソースコード](https://github.com/rails/rails/blob/01f58d62c2f31f42d0184e0add2b6aa710513695/activerecord/lib/active_record/inheritance.rb#L112-L153)

## サブクエリを使う

以下のようにして、where 句を使ってサブクエリが書ける。
ちなみに、squish は文字列を改行無しで出力するメソッド。

```ruby
sql = <<~SQL.squish
  EXIST(
    SELECT *
    FROM books
    WHERE books.author_id = authors.id
  )
SQL

Books.where(status: :published).where(sql)
```

### 参考
- [子レコードの条件で親レコードを絞り込みたいときはEXISTS句を活用しよう \- Qiita](https://qiita.com/jnchito/items/dac1e2fbb27ad2969376)
- [String](https://api.rubyonrails.org/classes/String.html#method-i-squish)
