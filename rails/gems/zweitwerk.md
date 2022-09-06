# Zweitwerk

## ディレクトリ構成と Zweitwerk の挙動について

### 概要

以下のように、payment というネームスペースに credit_concern とう module があるとする。

```shell
.
├── payment
│   └── credit_concern.rb
└── payment.rb
```

このとき、payment 内で credit_concern を include していても、credit_concern のメソッドを一度呼び出さないと uninitialized constant error になる。
なぜなら、Zweitwerk は最初にトップレベルだけを autoload して、第2階層移行はそのメソッドが呼び出されたときのコールバックとして、第2階層を見に行くから。

### 参考
