# CSV.foreach の col_sep

col_sep をカンマ区切りにすると CSV の行を each できる。また、タブにすると tsv にも対応できる。

```ruby
irb(main):005:0> csv = "a,b\nc,d\n"
=> "a,b\nc,d\n"
irb(main):011:0> file = File.write("test.csv", csv)
=> 8
irb(main):024:0> CSV.foreach("test.csv") { p _1  }
["a", "b"]
["c", "d"]
=> 8

irb(main):036:0> tsv = "a\tb\nc\td\n"
=> "a\tb\nc\td\n"
irb(main):037:0> file = File.write("test.tsv", tsv)
=> 8
irb(main):039:0> CSV.foreach("test.tsv", col_sep: "\t") { p _1  }
["a", "b"]
["c", "d"]
=> 8
irb(main):
```

## 参考

[Class: CSV \(Ruby 3\.1\.2\)](https://ruby-doc.org/stdlib-3.1.2/libdoc/csv/rdoc/CSV.html#method-c-foreach)
