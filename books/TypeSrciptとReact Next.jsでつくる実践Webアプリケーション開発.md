# TypeScriptとReact/Next.jsでつくる実践Webアプリケーション開発

## 学んだこと

### jQuery から SPA に至るまでの流れ
1. JavaScript 黎明期
  - 1995年
  - JS 登場時に Microsoft は JScript という JS に似た言語が動作するブラウザを実装した(Internet Explorer)
  - JScript は JS と非互換な部分が多かった
  - 当時は、JS はセキュリティ上問題があるものと認識されて、推奨されていなかった
  - Google Maps で状況が変わって Ajax を活用した Web アプリケーションが広まった
2. jQuery 全盛期
  - 以下の利点がある jQuery が普及した
    - クロスブラウザ対応
    - DOM 操作がかんたん
    - アニメーションがかんたん
    - 周辺ライブラリが充実
3. jQuery の人気低迷
  - jQuery は以下の問題から敬遠されることになる
    - グローバルスコープ汚染
    - DOM 操作が複雑になりやすい
    - 複数ページのWebアプリケーションを実装するしくみがない
    - ブラウザ間の非互換はそこまで問題にならなくなった
4. SPA の登場と MVC/MVVM の登場
