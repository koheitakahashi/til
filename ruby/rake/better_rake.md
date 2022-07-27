# 良い rake タスクの書き方

聞いたり、調べたりした良い rake タスクの書き方。

- description を書く
- 類似タスクはネームスペースで囲む
- ロジックをまとめたクラスを切り分ける
  - アプリの他の場所で再利用しやすい
  - テストができる
- タスクの進捗状況を詳細に伝える(エラーがあればメッセージを出す)
- ログファイルを使う
  - 標準出力だけだと、バックグラウンドなどで実行できない
- 処理するレコード件数が多かったり、多くのモデルのレコードを作成する場合は処理をモジュール化する
  - デバッグがしやすくなる

## 参考
- [Everything You Always Wanted to Know About Writing Good Rake Tasks \* But Were Afraid to Ask](https://edelpero.svbtle.com/everything-you-always-wanted-to-know-about-writing-good-rake-tasks-but-were-afraid-to-ask)
