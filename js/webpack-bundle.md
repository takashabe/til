# webpack のバンドル周りについて

## webpack 概要

webpackは `node_modules` や自分で書いたjsなどをまとめて一つのファイルにするために使われる。
これによりプロダクションで実行する場合などに、1つのファイルに依存関係を押し込める事ができる。goのbuildのような感じ。

## webpack の実行

まず以下のような `webpack.config.js` を用意し、`webpack [--config webpack.config.js]` すればconfig内で設定したOutput以下にバンドルされたファイルが配置される。

```js
var path = require('path');

module.exports = {
    entry : './src/app.js',
    output : {
        filename    : 'bundle.js',
        path        : path.resolve(__dirname, 'dist')
    }
};
```

## webpack-dev-server の場合

`webpack-dev-server` という開発用のwebサーバ+ファイル監視およびwebpackの自動実行が出来るモジュールがあり、開発時は大体これを使うと思われる。
ここで気をつけるのはバンドルされたファイルは、実ファイルに書き込まずメモル上に保持されるという点である。
したがって開発時のビルドとプロダクションのビルドはそれぞれ分けたほうが良い。最小構成は以下のようなものになると思う。

```json
// package.json
{
...
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server",
  },
...
}
```
