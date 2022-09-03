# WebAssembly

- Webブラウザ上で実行できる言語
- これにより、サーバーサイドの言語がクライアントでも実行できるようになる
- 現時点では DOM にアクセスできないので、JS と補完する形になる
- 目的の一つとしては、ブラウザ上でコードを高速に安全に実行すること
- WASM は仮想マシン上で実行するため、例えば OS と アプリケーションが通信することはできなかった
    - その通信ができるようにインターフェースを提供するのが WASI
- これにより、Ruby → WebAssembly にコンパイル → ブラウザで実行が可能になる

# 読んだ資料

- [Ruby 3\.2\.0 Preview 1 Released](https://www.ruby-lang.org/en/news/2022/04/03/ruby-3-2-0-preview1-released/)
- [WASI \|](https://wasi.dev/)
    - WASI について
- [RubyとWebAssemblyの関係についてわかる範囲でまとめる \| うなすけとあれこれ](https://blog.unasuke.com/2021/products-about-webassembly-and-ruby/)
    - Ruby の WASM におけるエコシステム?などが紹介されている
- [WebAssembly \| MDN](https://developer.mozilla.org/ja/docs/WebAssembly)
- [WebAssembly の概要 \- WebAssembly \| MDN](https://developer.mozilla.org/ja/docs/WebAssembly/Concepts)
    - WASM のコンセプトがまとめられていた
- [An Update on WebAssembly/WASI Support in Ruby \| by kateinoigakukun \| ITNEXT](https://itnext.io/final-report-webassembly-wasi-support-in-ruby-4aface7d90c9)
    - これが、発表者本人による Ruby Association Grant の報告書だけど、読んでもよくわからなかった
- [Artichoke Ruby Playground](https://artichoke.run/)
    - Rust による Ruby の実行環境。Rust は WASM にコンパイルできるので、WASM で Ruby を実行できるようにしたもの
