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
  - MVC を採用した FW の代表例は Backbonejs
  - MVVM を採用したライブラリの戦国時代
5. React の登場
  - React の特徴
    - 仮想 DOM など
      - 変更前後の仮想DOMを比較して、変更箇所のみに変更を適用する

### Next.js の特徴

複雑なフロントエンドの開発環境を簡便化できる。
基本的な思想としてはクライアント・サーバー間でコードを共有できるユニバーサルなJSアプリケーションを目指している。

- SPA/SSR/SSG の切り替えが容易
- ページルーティングがかんたん
- TSベース
- デプロイが簡単
- 学習コストが少ない
- webpack の設定を隠蔽
- ディレクトリベースの自動ルーティング機能
- コードの分割・統合

### コンポーネント指向のメリット

- 部品の再利用が容易(疎結合になる)
- グローバルを汚染しない
- コードの可読性工場
- テストが容易

目指すは、できる限り抽象的であること。

### 型アサーション

`as XXXX` で明示的に型を指定できる。
TypeScript が具体的な方を知ることができないケースで使う。しかし、実行時にエラーが起きる可能性があるので注意する。

### 型エイリアス

同じ型を何度も利用する場合に `type 型名 = 型` で、その型を複数回参照できる。
interface は後から拡張可能であるが、型エイリアスはそうはいかない。

### Enum 型

```typescript
enum Direction = {
  'Up': 0,
  'Down': 1,
}

let direction: Direction = Direction.left
console.log(direction)
```

### ジェネリック型

型を抽象化し、外部から具体的な型を指定できる機能。

```typescript
// T はクラス内で利用する仮の方の名前
class Queue<T> {
  private array: T[] = []

  push(item: T) {
    this.array.push(item)
  }

  pop(): T | undefined {
    return this.array.shift()
  }
}

// T が number であることを外部から強制
const queue = new Queue<number>()
queue.push(111)
queue.push(112)
queue.push('hoge')

// T が string であることを外部から強制
const queueStr = new Queue<string>();
queueStr.push('hoge')
queueStr.push('fuga')
queueStr.pop(111)
```

### Union 型 / Intersection 型

少し複雑な方を表現したいときに使う。
指定した複数の方の和集合を意味する Union 型。積集合 Intersection 型。

```typescript
type Identity = {
  id: number | string;
  name: string;
}

type Contact = {
  name: string;
  email: string;
  phone: string;
}

type IdentityOrContact = Identity | Contact;

const id: IdentityOrContact = {
  id: '111',
  name: "Takuya"
}

const contact: IdentityOrContact = {
  name: "Takuya",
  email: "test@example.com",
  phone: '01235'
}

type Employee = IdentityOrContact & Contact

const employee: Employee = {
  id: '111',
  name: "Takuya",
  email: "test@exampmle.com",
  phone: '01235'
}

// これはエラーになる
const employee2: Employee = {
  email: "test@exampmle.com",
  phone: '01235'
}
```

### never 型が有効なケース

条件分岐に漏れがないことを保証するケース。

### 型ガード

if文やswitch 文の条件分岐にて型チェックを行った際、条件分岐ブロック以降は変数の型を絞り込まれる推論が行われる。

```typescript
function addOne(value: number | string) {
    if (typeof value === 'number') {
        return value + 1;
    } else {
        return value;
    }
}
```

### keyof オペレーター

その方が持つ各プロパティの型の Union 型を返す。o


```typescript
interface User {
  name: string;
  age: number;
  email: string;
}

type UserKey = keyof User // 'name' | 'age' | 'email' のいずれかになる

function printUserKey(str: UserKey) {
  console.log(str)
}

printUserKey('name')
printUserKey('hoge') // これはエラーになる
```

### React がレンダリングするまでの流れ

- JSX で書かれたコンポーネントを、webpack が JS コードに変換する
- JS コードをブラウザが読み込み実行する
- JS がブラウザの表示内容を書き換えるときに、仮想DOMを構築する
- 前回構築した仮想DOMの差分と比較して差分があるところだけを更新する
