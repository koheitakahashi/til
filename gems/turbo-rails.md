# Websocket版 TurboStream の内部実装

## boradcast_xxx_to

controller などに以下のように、broadcast_replace_to を書くと、何が呼び出されるのかを見ていきます。

```ruby
# Payment モデルがあったとして
@payment.broadcast_replace_to(
      'payments',
      target: 'total_amount',
      partial: 'monthly_expenses/total_amount',
      locals: { total_amount: }
)
```

まず、`broadcast_replace_to` は以下で定義されています。

```ruby
# https://github.com/hotwired/turbo-rails/blob/main/app/channels/turbo/streams/broadcasts.rb#L12-L14
# turbo-rails/app/channels/turbo/streams/broadcasts.rb

def broadcast_replace_to(*streamables, **opts)
  broadcast_action_to(*streamables, action: :replace, **opts)
end
```

上記で、呼び出されている `broadcast_action_to` は以下で定義されています。
やっていることを端的にまとめると、引数から HTML を作成して、それを指定した stream に broadcast するということをやっています。
このとき、`broadcast_stream_to` は ActionCable の broadcast メソッドを呼び出しているだけです。([該当箇所](https://github.com/hotwired/turbo-rails/blob/50402466cd85f27a8515b3865797d12ac741c26d/app/channels/turbo/streams/broadcasts.rb#L79-L81))

```ruby
# https://github.com/hotwired/turbo-rails/blob/50402466cd85f27a8515b3865797d12ac741c26d/app/channels/turbo/streams/broadcasts.rb#L36-L40
# turbo-rails/app/channels/turbo/streams/broadcasts.rb

def broadcast_action_to(*streamables, action:, target: nil, targets: nil, **rendering)
  broadcast_stream_to(*streamables, content: turbo_stream_action_tag(action, target: target, targets: targets, template:
    rendering.delete(:content) || rendering.delete(:html) || (rendering.any? ? render_format(:html, **rendering) : nil)
  ))
end
```

###  boradcast_xxx_to のまとめ

broadcast_xxx_to が実際にやっていることは以下です。

- 引数から、HTML を組み立てる
- それを ActionCable の broadcast メソッドを使って、指定した stream に broradcast する

## turbo_stream_from "xxx"

turbo_stream_from "xxx" の中身を追っていきます。

```
<%= turbo_stream_from "payments" %>
```

上記のように view ファイルに `turbo_stream_from` を書くと以下の HTML が追加されます。

```html
<turbo-cable-stream-source
  channel="Turbo::StreamsChannel"
  signed-stream-name="InRvdGF...."
>
</turbo-cable-stream-source>
```

このカスタムエレメントはどこで定義されて、これがあることで何が起きるのかを追っていきます。
まず、`turbo_stream_from` は以下に定義されています。

channel を明示的に渡さない場合には、デフォルトで `Turbo::StreamsChannel` がセットされます。
signed-stream-name には、stream の名前がエンコードされたものが入っています。これにより、他のストリームからデータを受信しないようにしています。

```ruby
# https://github.com/hotwired/turbo-rails/blob/d4e26fec0ed02e06cd3ab26882dc234fccd2972a/app/helpers/turbo/streams_helper.rb
# turbo-rails/app/helpers/turbo/streams_helper.rb

def turbo_stream_from(*streamables, **attributes)
  attributes[:channel] = attributes[:channel]&.to_s || "Turbo::StreamsChannel"
  attributes[:"signed-stream-name"] = Turbo::StreamsChannel.signed_stream_name(streamables)

  tag.turbo_cable_stream_source(**attributes)
end
```

-----------

そして、`<turbo-cable-stream-source>` というカスタムエレメントは以下で定義されています

カスタムエレメントがドキュメントに接続された要素に追加されるタイミングで、subscription を作成します。
このとき、subscribe される channel は、このカスタムエレメントの attribute になっている channel です。なので、デフォルトでは `Turbo::StreamsChannel` がセットされます。

subscription されると同時に consumer がデータを受け取ったタイミングで `message` イベントを発火するように定義しています。

```js
// https://github.com/hotwired/turbo-rails/blob/main/app/javascript/turbo/cable_stream_source_element.js#L5
// turbo-rails/app/javascript/turbo/cable_stream_source_element.js

...

class TurboCableStreamSourceElement extends HTMLElement {
  async connectedCallback() {
    connectStreamSource(this)
    this.subscription = await subscribeTo(this.channel, { received: this.dispatchMessageEvent.bind(this) })
  }

...

  dispatchMessageEvent(data) {
    const event = new MessageEvent("message", { data })
    return this.dispatchEvent(event)
  }

  get channel() {
    const channel = this.getAttribute("channel")
    const signed_stream_name = this.getAttribute("signed-stream-name")
    return { channel, signed_stream_name, ...snakeize({ ...this.dataset }) }
  }
}

customElements.define("turbo-cable-stream-source", TurboCableStreamSourceElement)
```

----

ちなみに、上記で呼び出されている `subscribeTo` を定義しているのは以下になります。

つまり、subscribeTo が呼ばれた時点で  consumer の create までやっているということになります。
このとき `createConsumer` の関数は、actioncable から import した関数を使っているのが分かります。

```js
// https://github.com/hotwired/turbo-rails/blob/main/app/javascript/turbo/cable.js
// turbo-rails/app/javascript/turbo/cable.js

let consumer

export async function getConsumer() {
  return consumer || setConsumer(createConsumer().then(setConsumer))
}

export function setConsumer(newConsumer) {
  return consumer = newConsumer
}

export async function createConsumer() {
  const { createConsumer } = await import(/* webpackChunkName: "actioncable" */ "@rails/actioncable/src")
  return createConsumer()
}

export async function subscribeTo(channel, mixin) {
  const { subscriptions } = await getConsumer()
  return subscriptions.create(channel, mixin)
}
```

-------

`turbo-rails/app/javascript/turbo/cable_stream_source_element.js` のコードに戻って、今度は `connectStreamSource` を見ていきます。

`connectStreamSource` は、turbo-rails ではなく、turbo で定義されている関数で以下になります。

source には、`<turbo-cable-stream-source>` のカスタムエレメントが渡されます。
つまり、そのカスタムエレメントに対して、message イベントに対するハンドラを定義しています。message イベントが発火した時には、サーバーから broadcast された HTML 片を document の要素に appendChild する処理が呼び出されます。

```js
// https://github.com/hotwired/turbo/blob/e30d36bf56d020d63d87a8350d9d0361edd9b804/src/observers/stream_observer.ts#L32-L36
// turbo/src/observers/stream_observer.ts

connectStreamSource(source: StreamSource) {
  if (!this.streamSourceIsConnected(source)) {
    this.sources.add(source)
    source.addEventListener("message", this.receiveMessageEvent, false)
  }
}
```

ちなみに appendChild する処理は以下になります。
`turbo/src/observers/stream_observer.ts` で呼び出される `receiveMessageEvent` は最終的には以下のコードを実行することとなります。
https://github.com/hotwired/turbo/blob/700c921838d7e22b18c19200cfa6cc39cb4ad09d/src/core/session.ts#L106-L108
