# PIXI.js

## 開発環境の構築

### プロジェクトのホームディレクトリの作成
```bash
$ cd ~/dev/
$ mkdir -p ~/dev/pixi/frontend/
$ mkdir -p ~/dev/pixi/backend/
```

## node.jsのバージョンの確認
```bash
$ node --version
```

## 必要なパッケージのインストール
```bash
$ npm install path
$ npm install url
$ npm install dotenv
```

## node.jsパッケージのセットアップ
```bash
$ cd ~/dev/pixi/frontend/
$ npm init
$ ls
$ less package.json
```

## TypeScript関連モジュールのインストール
```bash
$ npm i --save-dev typescript tslint tslint-config-airbnb typedoc
$ less package.json
```

## TypeScriptモジュールの実行
```bash
$ cd ~/dev/pixi/frontend/
$ code .
```

`tsconfig.json`
```json
{
    "compilerOptions": {
        "target": "es5"
    },
    "files": [
        "index.ts"
    ]
}
```

`index.ts`
```ts
function sum(v1: number, v2: number): number {
    return v1+v2;
}

const n = 1;
const s = 2;

const result = sum(n, s);
console.log(result);
```

```bash
$ /node_modules/.bin/tsc -p .
$ node index.js
```

## tslintモジュールの実行と設定
`tslint.json`
```json
{
    "defaultSeverity": "error",
    "extends": [
        "tslint-config-airbnb"
    ]
}
```

```bash
$ ./node_modules/.bin/tslint -p .
ERROR: /home/rsl/dev/pixi/frontend/index.ts:2:14 - missing whitespace
ERROR: /home/rsl/dev/pixi/frontend/index.ts:2:15 - missing whitespace
ERROR: /home/rsl/dev/pixi/frontend/index.ts:9:21 - file should end with a newline
```

## typedocモジュールの実行と設定
`tsconfig.json`に下記を追加する。

`tsconfig.json`
```
...（略）...
    "typedocOptions": {
        "name": "test_npm",
        "mode": "file",
        "out": "./docs"
    }
```

```bash
$ ./node_modules/.bin/typedoc -p .
（エラー）
```

## scriptsの設定によるコンパイル設定
`package.json`に下記を追加する。

`package.json`
```
...（略）...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "tsc": "tsc -p .",         # 追加
    "tslint": "tslint -p .",   # 追加
    "typedoc": "typedoc -p ."  # 追加
  },
...（略）...
```

下記でスクリプトを実行できる。
```bash
$ npm run tsc
$ npm run tslint
$ npm run typedoc
```

## ファイルのバンドル
```bash
$ npm i --save-dev webpack webpack-cli webpack-dev-server ts-loader
```

`webpack.config.js`
```
const path         = require('path');

module.exports = (env, argv) => {
  return {
    mode: 'production',
    entry: {
      index: path.join(__dirname, 'index.ts'),
    },

    output: {
      path: path.join(__dirname, 'www'),
      filename: 'test_npm.js',
      library: 'test_npm',
      libraryTarget: 'umd'
    },

    module: {
      rules: [
        {
          test: /\.ts$/,
          use: [
            { loader: 'ts-loader' }
          ]
        }
      ]
    },
  }
};
```

`package.json`
```
...（略）...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "tsc": "tsc -p .",
    "tslint": "tslint -p .",
    "typedoc": "typedoc -p .", # カンマを追加
    "webpack": "webpack"       # 追加
  },
...（略）...
```

```bash
$ npm run webpack
$ ls www/
$ less www/test_npm.js
```

## 開発用Webサーバの構築
```bash
$ npm i --save-dev webpack-dev-server
```

`webpack.config.js`に下記を追加する。

`webpack.config.js`
```
const webpack      = require('webpack');  # 追加
const path         = require('path');
...（略）...
    devServer: {
        static: {
            directory: 'www',
        },
        port: 8080,
      },

    plugins: [
        new webpack.ProvidePlugin({
            process: 'process/browser',
        }),
    ]
...（略）...
```

※`webpack-dev-server`のバージョンの違いにより、`contentBase`の代わりに`static`を使う必要がある。


`www/index.html`
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <script type='text/javascript' src='/test_npm.js'></script>
    </head>
    <body></body>
</html>
```

`package.json`
```
...（略）...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "tsc": "tsc -p .",
    "tslint": "tslint -p .",
    "typedoc": "typedoc -p .",
    "webpack": "webpack",           # カンマを追加
    "server": "webpack-dev-server"  # 追加
  },
...（略）...
```

```bash
$ npm run server
```

http://localhost:8080/ にアクセスし、デベロッパーツールのコンソールに`3`と表示されればOK。

## PIXI.jsのインストール
```bash
$ npm i --save pixi.js@4.8.3
$ npm i --save-dev @types/pixi.js@4.8.4
$ less pacakeg.json
```

`index.ts`
```ts
import * as PIXI from 'pixi.js';

window.onload = () => {
    const app = new PIXI.Application({ width: 400, height: 200 });
    document.body.append(app.view);

    const text = new PIXI.Text('Hello World!');
    text.style.fill = '#ffffff';
    app.stage.addChild(text);
};
```

```bash
$ npm run tsc
$ npm run webpack
$ npm run server
```

#### 参考
- Smith，佐藤 英一，『HTML5 ゲーム開発の教科書　スマホゲーム制作のための基礎講座』，ボーンデジタル，2019．
- [webpack-dev-server起動時のエラーと解決法 #npm - Qiita](https://qiita.com/yosyosyoyoyo/items/84b921d8baa73b232755)
- [Webpack5ではprocessはundefinedになる](https://zenn.dev/szgk/articles/2d0843136d2fa8)
