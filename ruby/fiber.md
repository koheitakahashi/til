# Fiber

## Fiber とは

- ノンプリエンプティブな軽量スレッド
  - スレッドと同じようなもの
  - ノンプリエンプティブというのは、マルチタスクを OS に任せずに開発者が明示的にスケジューリングするタスクのスケジューリング方式のこと
  - つまり、処理を途中で止め、他の Fiber を実行して、その後にまた戻って続きを実行することができる

## Fiber と Thread の違いコード例

Thread は Fiber のように途中で処理を他の Thread に渡したら、それの続きから再度実行することができない。
Fiber の場合、yield で親の Fiber にコンテキストを切り替えて、その親の処理が終わったら、また処理を実行することができる。

```ruby
# Thread の場合
n = 0
m = 10

Thread.new do
  5.times do
    n +=1
    Thread.pass
    m -= 1
  end
  exit
end

5.times do
  p "n: #{n}, m: #{m}"
  Thread.pass
end
```

```ruby
# Fiber の場合
n = 0
m = 10

f = Fiber.new do
  5.times do
    n += 1
    Fiber.yield
    m -= 1
  end
end

5.times do
  f.resume
  p "n: #{n}, m: #{m}"
end
```

## 読んだ資料

- [class Fiber \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/3.1/class/Fiber.html)
- [【Ruby】Fiber（ファイバー）を理解する \- Qiita](https://qiita.com/k-penguin-sato/items/baec25479351ff1b6469#:~:text=Fiber(%E3%83%95%E3%82%A1%E3%82%A4%E3%83%90%E3%83%BC)%E3%81%A8%E3%81%AF%EF%BC%9F,%E3%81%9B%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)
- [Fiber Schedulerの使い方 \- マイペースなRailsおじさん](https://ytnk531.hatenablog.com/entry/2020/10/24/183321#:~:text=Fiber%20Scheduler%E3%81%AF%E3%80%81Ruby3%E3%81%A7,%E3%82%89%E3%82%8C%E3%82%8B%E3%81%A8%E3%81%84%E3%81%86%E8%A9%B1%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82)
- [Ruby 3 adds Scheduler Interface for Fibers](https://blog.kiprosh.com/ruby-fiber-schedular/)
- [class Fiber::SchedulerInterface \- Documentation for Ruby 3\.2](https://docs.ruby-lang.org/en/master/Fiber/SchedulerInterface.html)
- [プロと読み解く Ruby 3\.0 NEWS \- クックパッド開発者ブログ](https://techlife.cookpad.com/entry/2020/12/25/155741)
