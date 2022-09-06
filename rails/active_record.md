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

## ポリモーフィック関連

### 解決したい問題

トラックと移動手段、ボートと移動手段というような、2つのモデルに紐づく同じリソースがあったとき、普通に関連付けを実装すると名前衝突が起こる。

そもそも、移動手段は多態であるため、それを適切に表現するためにポリモーフィック関連付けがある。

### 解決策

Rails ガイドにあるように、imageable などの抽象的な名前として、社員・製品に関連付ける。

[Active Record の関連付け \- Railsガイド](https://railsguides.jp/association_basics.html#%E3%83%9D%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91)

### メリット

- コードが DRY に保てる
- 多態を表すことができて、新しいテーブルを追加する必要がなくなる
- 関連付けが容易に作れる

### デメリット

- 外部キー制約を持たない
- 初学者の人には慣れないかもしれない

### 参考

- [Active Record の関連付け \- Railsガイド](https://railsguides.jp/association_basics.html#%E3%83%9D%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%95%E3%82%A3%E3%83%83%E3%82%AF%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91)
- [Polymorphic Associations in Rails \- DEV Community](https://dev.to/jblengino510/polymorphic-associations-in-rails-167o)
- [Changing a polymorphic\_type in Rails — Development \(2022\)](https://shopify.engineering/changing-polymorphic-type-rails#:~:text=Polymorphic%20relationship%20in%20Rails%20refers,having%20to%20define%20one%20association.))
