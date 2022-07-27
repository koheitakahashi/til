# プログレスバー

簡単なプログレスバーの実装。キャリッジリターンを使うのがポイント。
sleep 挟まないと、標準出力に何も表示されない。

```ruby
(1..100).each do |num|
  print "処理中...#{((num/100.to_f)*100).round(2)}%\r"
  sleep(0.01)
  STDOUT.flush
end
```
