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
    "target": "es5",
    "module": "esnext",
    "lib": [
      "esnext",
      "dom"
    ],
    "baseUrl": "./src",
    "outDir": "./lib-ts",
    "declaration": true,
    "sourceMap": true,
    "allowJs": false,
    "forceConsistentCasingInFileNames": true,
    "allowSyntheticDefaultImports": false,
    "moduleResolution": "node",
    "strict": true,
    "alwaysStrict": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": false,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "preserveConstEnums": false
  },
  "compileOnSave": true,
  "include": [
    "src/**/*"
  ],
  "exclude": [
    ".git",
    "node_modules",
    "lib"
  ],
  "typedocOptions": {
    "name": "tower-diffence",
    "mode": "file",
    "out": "./docs",
    "gitRevision": "develop",
    "ignoreCompilerErrors": true,
    "includeDeclarations": true,
    "excludeExternals": true,
    "excludePrivate": false,
    "excludeProtected": false,
    "excludeNotExported": false,
    "preserveConstEnums": true
  }
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

#### 参考
- Smith，佐藤 英一，『HTML5 ゲーム開発の教科書　スマホゲーム制作のための基礎講座』，ボーンデジタル，2019．
