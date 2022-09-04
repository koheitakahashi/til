# Web3 と Ruby について

## Ethereum とは

- 暗号通貨の1種
- 暗号通貨とはブロックチェーン技術をつかって、取引内容を逐次記録してチェーンのようにつなげた通貨のこと
- ブロックチェーン技術は取引の追加は容易だが削除は難しいため高い安全性がある
    - チェーンとは親のブロックを暗号的に参照すること
- ビットコインと何が違うの?
    - ビットコインは単なる決済ネットワーク
    - イーサリアムはプログラマブルであるため、できることの幅が広い
        - スマートコントラクトもその一つ
- 分散型システムであり、ボランティアが運営している数千台のノードを使っている
    - そのため、システムがダウンしづらい
- マイニングとは
    - ブロックを追加する際に、暗号をブルートフォースで解かなければならない
    - そのため、計算処理が必要
    - その計算処理のために、自分のコンピュータのリソースを開放して、その報酬としてイーサをもらう

## NFT とは

- 非代替トークン
- 非代替な特性を持つもの
- 1点ものの芸術品やシリアル番号が振られたコレクションアイテム、証明書などの用途で利用される
- NFT は [ERC-721](https://eips.ethereum.org/EIPS/eip-721) で明確に定義されていて、インターフェースが決められている
- そのインターフェースを実装したコントラクトを作れば、それは NFT

### スマートコントラクトとは

- 特定の条件が満たされた場合に決められた手続きを実行する仕組み
- スマートコントラクトの記述言語として Solidity がある

## Ruby で Ethereum を使う

- gethとは
    - Go Ethereum という Ethereum のブロックチェーンネットワークに参加するための Ethereum クライアント
- https://github.com/kurotaky/sample-rails-dapp
    - Solidity で実行されたファイルをコンパイルして、Rails 側からは NFT を発行するときに、そのスマートコントラクトを実行する流れ
    - ブロックチェーンに実行されたスマートコントラクトが刻まれる

## Web3 の実例

- [Nouns DAO](https://nouns.wtf/) というものがある

# 参考資料

- [WEB+DB PRESS vol 130](https://www.amazon.co.jp/dp/B0B8MY1BWR)
    - これが分かりやすかった
- [Intro to Ethereum \| ethereum\.org](https://ethereum.org/ja/developers/docs/intro-to-ethereum/)
- [本書について \- Ethereum入門](https://book.ethereum-jp.net/)
- [https://cryptozombies.io/](https://cryptozombies.io/)
- [https://buildspace.so/](https://buildspace.so/)
