# Active Hash

## 思ったこと

以下のようにトップレベルで ActiveRecord::Base に extend している。

```ruby
# https://github.com/active-hash/active_hash#referencing-activehash-objects-from-activerecord-associations
ActiveRecord::Base.extend ActiveHash::Associations::ActiveRecordExtensions

class Country < ActiveHash::Base
end

class Person < ActiveRecord::Base
  belongs_to_active_hash :country
end
```

しかし、Rails の場合、基本的には ApplicationRecord が Model 層のクラスの抽象クラスになるため、以下のようにしたほうが良さそう。

```ruby
class ApplicationRecord < ActiveRecord::Base
  extend ActiveHash::Associations::ActiveRecordExtensions
end
```
