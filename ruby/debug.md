# debug.gem

## VSCode で debug.gem を使う方法

- `ruby-debug-ide` をインストール
- `debase` をインストール
- launch.json に以下を

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Run rails server",
      "type": "Ruby",
      "request": "launch",
      "program": "${workspaceRoot}/bin/rails",
      "args": [
        "server"
      ]
    }
  ]
}
```

- debug モードで起動
- VSCode 上でブレークポイントを挟む

## 参考

- [Debugging Ruby On Rails with VSCode BREAKPOINTS \| Intro To Rails 7 Part 25 \- YouTube](https://www.youtube.com/watch?v=SF4HyCz8a70)
- [ruby/debug: Debugging functionality for Ruby](https://github.com/ruby/debug)
