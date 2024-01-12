# Webシステム開発環境の構築

## プロジェクトのホームディレクトリの作成
```bash
$ cd /usr/local/src/
$ sudo mkdir dev/
$ sudo chmod 777 dev/
$ ls -l
$ mkdir -p dev/app/
$ mkdir -p dev/app/frontend/
$ mkdir -p dev/app/backend/
```

## フロントエンド開発の準備

### フロントエンドのコンテナの構築
```bash
$ systemctl status docker
# Dockerが起動していることを確認する。
$ cd /usr/local/src/dev/app/frontend/
$ code .
```

1. VSCodeの左下の**Manage > Command Palette**を開く。
   1. 「containers」で検索する。
   2. 「Dev Containers: Open Folder in Container」を選択する。
   3. 現在のディレクトリ`/usr/local/src/dev/app/frontend/`が表示されていることを確認し、**開く**ボタンをクリックする。
   4. コンテナー構成テンプレートとして「Node.js」を選択する。
   5. バージョン「18」を選択する。
   6. **OK**ボタンをクリックする。

### Next.jsのアプリケーションのインストール
1. VSCodeの上部メニューから**ターミナル > 新しいターミナル**を開く。

VSCodeのターミナルで下記コマンドを実行する。
```bash
$ yarn create next-app frontend --ts --esling
# 何度か質問されるがデフォルトのまま進める。
# ...（3分程度）...
$ mv frontend/* .
$ mv frontend/.* .
$ rmdir frontend/
```

### フロントエンド環境の動作確認
VSCodeのターミナルで下記コマンドを実行する。
```bash
$ yarn dev
```

ブラウザで下記URLにアクセスし、ウェルカムページが表示されれば成功。
- http://localhost:3000

### gitignoreの設定
`.gitignore`ファイルに下記を追加する。

`/workspaces/frontend/.gitignore`
```txt
...（略）...
#Vscode
.vscode/
...（略）...
```

### next.config.jsの編集
`next.config.js`を下記のように編集する。

`/workspaces/frontend/next.config.js`
```js
/** @type {import('next').NextConfig} */
const nextConfig = {}

module.exports = {
    async rewrites() {
        return [
            {
                source: '/api/:path*',
                destination: 'http://host.docker.internal:8000/api/:path*/',
            },
        ]
    },
};
```

### globals.cssの設定
`app/globals.css`の内容を削除する。

`/workspaces/frontend/app/globals.css`
```css
```

### 機能拡張
VSCodeのターミナルで下記コマンドを実行する。
```bash
$ yarn add @mui/material @emotion/react @emotion/styled # React UI tools
$ yarn add @mui/x-data-grid # ReactGrid
$ yarn add axios #axios
$ yarn add swr # SWR
```

## バックエンド開発の準備

### バックエンドのコンテナの構築
```bash
$ cd /usr/local/src/dev/app/backend/
$ code .
```

1. VSCodeの左下の**Manage > Command Palette**を開く。
   1. 「containers」で検索する。
   2. 「Dev Containers: Open Folder in Container」を選択する。
   3. 現在のディレクトリ`/usr/local/src/dev/app/backend/`が表示されていることを確認し、**開く**ボタンをクリックする。
   4. コンテナー構成テンプレートとして「Python 3」を選択する。
   5. バージョン「3.12-bullseye」（最新版の安定版）を選択する。
   6. **OK**ボタンをクリックする。

### 環境設定
VSCodeのターミナルで下記コマンドを実行する。
```bash
$ echo -e 'djangorestframework\npsycopg2-binary' > requirements.txt
$ pip install -r requirements.txt
$ pip freeze > requirements.lock
$ cat requirements.lock
```

### Dockerの設定
`devcontainer.json`の下記の箇所を下記のように置き換える。

`/workspaces/backend/.devcontainer/devcontainer.json`
```json
...（略）...
	// Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
	"image": "mcr.microsoft.com/devcontainers/python:1-3.12-bullseye", # <- カンマを付ける
...（略）...
	// Use 'postCreateCommand' to run commands after the container is created.
	"postCreateCommand": "pip3 install --user -r requirements.lock", # <- コメントを解除し、txtをlockに変える
...（略）...
```
