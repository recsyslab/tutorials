# Webシステム開発環境の構築

## フロントエンド開発の準備

### プロジェクトのホームディレクトリの作成
```bash
$ sudo mkdir ~/dev/
$ sudo chmod 777 ~/dev/
$ ls -l
$ mkdir -p ~/dev/app/frontend/
$ mkdir -p ~/dev/app/backend/
```

### Node.jsのインストール
```bash
$ curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
$ sudo apt install nodejs
$ node --version
# v18.x以上であることを確認する。
```

#### 参考
- [Windows、macOS、LinuxにNode.jsとnpmをインストールする方法](https://kinsta.com/jp/blog/how-to-install-node-js/)

### yarnのインストール
```bash
$ sudo mkdir -p /usr/local/share/keyrings/
$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo gpg --dearmor -o /usr/local/share/keyrings/yarn-archive-keyring.gpg
$ echo "deb [signed-by=/usr/local/share/keyrings/yarn-archive-keyring.gpg] https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
$ sudo apt update
$ sudo apt install yarn
```

#### 参考
- [apt-keyを使わないサードパーティーリポジトリからのパッケージのインストール方法 #Ubuntu - Qiita](https://qiita.com/SolKul/items/5b87bcf325db63b0ea28)

### Next.jsのアプリケーションのインストール
```
$ cd ~/dev/app/frontend/
$ yarn create next-app frontend --ts --esling
# 何度か質問されるがデフォルトのまま進める。
# ...（3分程度）...
$ mv frontend/* .
$ mv frontend/.* .
$ rmdir frontend/
```

### フロントエンド環境の動作確認
```bash
$ yarn dev
```

ブラウザで下記URLにアクセスし、ウェルカムページが表示されれば成功。
- http://localhost:3000

### VSCodeの起動
```bash
$ cd ~/dev/app/frontend/
$ code .
```

### gitignoreの設定
`.gitignore`ファイルに下記を追加する。

`~/dev/app/frontend.gitignore`
```txt
...（略）...
#Vscode
.vscode/
...（略）...
```

### next.config.jsの編集【保留】
`next.config.js`を下記のように編集する。

`~/dev/app/frontend/next.config.js`
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

`~/dev/app/frontend/app/globals.css`
```css
```

### 機能拡張
```bash
$ yarn add @mui/material @emotion/react @emotion/styled # React UI tools
$ yarn add @mui/x-data-grid # ReactGrid
$ yarn add axios #axios
$ yarn add swr # SWR
```










## 仮想環境の構築

### rsl-base環境からのインストール済みパッケージ情報の出力
```bash
$ source ~/venv/rsl-base/bin/activate
(rsl-base) $ pip freeze > ~/venv/rsl-base_requirements.txt
(rsl-base) $ deactivate
```

### rsl-web環境でのパッケージ情報の読込み
```bash
$ python3.12 -m venv ~/venv/rsl-web
$ source ~/venv/rsl-web/bin/activate
(rsl-web) $ pip install --upgrade pip
(rsl-web) $ pip install -r ~/venv/rsl-base_requirements.txt
(rsl-web) $ pip freeze
(rsl-web) $ deactivate
```

## Webシステム開発関連パッケージのインスト―ル
```bash
$ source ~/venv/rsl-web/bin/activate
(rsl-web) $ pip install django
(rsl-web) $ pip install django-leaflet
(rsl-web) $ export CPLUS_INCLUDE_PATH=/usr/include/gdal
(rsl-web) $ export C_INCLUDE_PATH=/usr/include/gdal
(rsl-web) $ apt list --installed | grep libgdal-dev
libgdal-dev/jammy,now 3.4.1+dfsg-1build4 amd64 [インストール済み]
# libgdal-devのバージョンを確認する。
(rsl-web) $ pip install gdal==3.4.1 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
# ...（1分程度）...
(rsl-web) $ pip install djangorestframework-gis # RESTful APIに必要
(rsl-web) $ pip install django-filter # RESTful APIに必要
(rsl-web) $ pip install markdown # RESTful APIに必要
(rsl-web) $ pip install django-bootstrap5
(rsl-web) $ pip install django-allauth
(rsl-web) $ pip install django-cleanup
```

## インストール済みパッケージ一覧の確認
```bash
(rsl-web) $ pip freeze
```

## rsl-web仮想環境のディアクティベート
```bash
(rsl-web) $ deactivate
$
# プロンプトが元に戻ればOK
```

## Django環境の動作テスト

### Djangoプロジェクトの作成
```bash
(rsl-web) $ cd
(rsl-web) $ mkdir django-test/
(rsl-web) $ cd django-test/
(rsl-web) $ django-admin startproject test_project
(rsl-web) $ ls
# `test_project/`が正しく生成されればOK
```

### Djangoアプリケーションの作成
```bash
(rsl-web) $ cd test_project/
(rsl-web) $ python manage.py startapp test_app
(rsl-web) $ ls
# `test_app/`が正しく生成されればOK
```

### Djangoプロジェクトの削除
```bash
(rsl-web) $ cd
(rsl-web) $ rm -rf django-test/
(rsl-web) $ deactivate
```

#### 参考
- 株式会社オープントーン，佐藤大輔，伊東直喜，上野啓二，『実装で学ぶフルスタックWeb開発 エンジニアの視野と知識を広げる「一気通貫」型ハンズオン』，翔泳社，2023．


