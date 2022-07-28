# ActiveRecord
## abstract_class の使い時

STI を使っているとき、その抽象クラスで abstract_class = true にすると table と model が1対1対応しないことを明示できる。

### 参考

- [abstract_class のソースコード](https://github.com/rails/rails/blob/01f58d62c2f31f42d0184e0add2b6aa710513695/activerecord/lib/active_record/inheritance.rb#L112-L153)
