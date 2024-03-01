# Webシステム開発環境の構築

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

### Webシステム開発関連パッケージのインスト―ル
```bash
$ source ~/venv/rsl-web/bin/activate
(rsl-web) $ pip install djangorestframework
(rsl-web) $ pip install djangorestframework-simplejwt
(rsl-web) $ pip freeze
(rsl-web) $ deactivate
```

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
$ cd ~/dev/app/frontend/
$ yarn dev
```

ブラウザで http://localhost:3000 にアクセスし、ウェルカムページが表示されれば成功。


### VSCodeの起動
```bash
$ cd ~/dev/app/frontend/
$ code .
```

### gitignoreの設定
`.gitignore`ファイルに下記を追加する。

`~/dev/app/frontend/.gitignore`
```txt
...（略）...
#Vscode
.vscode/
...（略）...
```

### next.config.jsの編集
`next.config.js`（または`next.config.mjs`）を下記のように編集する。

`~/dev/app/frontend/next.config.mjs`
```js
/** @type {import('next').NextConfig} */
const nextConfig = {}

module.exports = {
    async rewrites() {
        return [
            {
                source: '/api/:path*',
                destination: 'http://localhost:8000/api/:path*/',
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
$ yarn add @mui/icons-material # Material UI
$ yarn add mui-file-input # ファイルアップロード
$ yarn add react-hook-form # React Hook Form
$ yarn add axios #axios
$ yarn add swr # SWR
$ yarn add pixi.js@4.8.3        # PIXI.js
$ yarn add @types/pixi.js@4.8.4 # PIXI.js
$ less ~/dev/app/frontend/package.json
$ less ~/dev/app/frontend/yarn.lock
```

## バックエンド開発の準備

### Djangoプロジェクトの設定
```bash
$ source ~/venv/rsl-web/bin/activate
(rsl-web) $ cd ~/dev/app/backend/
(rsl-web) $ django-admin startproject config .
```

### VSCodeの起動
```bash
$ cd ~/dev/app/backend/
$ code .
```

### gitignoreの設定
```bash
$ echo '__pycache__/' > .gitignore
```

### Pythonの設定
```bash
$ mkdir config/settings/
$ mv config/settings.py config/settings/base.py
$ echo 'from .base import *' > config/settings/development.py
```

`development.py`を下記のように編集する。

`~/dev/app/backend/config/settings/development.py`
```py
from .base import *
import os

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'app',
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': '',
        'PORT': '',
    }
}
```

### Djangoサーバの起動
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# CREATE ROLE rsl WITH LOGIN PASSWORD '【パスワード】';
postgres=# CREATE DATABASE app ENCODING 'UTF8';
postgres=# \q
```

```bash
$ source ~/venv/rsl-web/bin/activate
(rsl-web) $ export DB_USER=rsl
(rsl-web) $ export DB_PASSWORD=【パスワード】
(rsl-web) $ python manage.py runserver --settings config.settings.development
```

ブラウザで http://localhost:8000/ にアクセスし、「The install worked successfully! Congratulations!」と表示されれば成功。

### Djangoの設定

`~/dev/app/backend/config/settings/base.py`
```py
...（略）...
ALLOWED_HOSTS = ['*']             # 修正
...（略）...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'rest_framework',             # 追加
]
...（略）...
LANGUAGE_CODE = 'ja-jp'           # 修正
...（略）...
# 以下を追加
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        }
    },
    'loggers': {
        'django.db.backends': {
            'level': 'DEBUG',
            'handlers': ['console'],
        }
    }
}
```

ブラウザで http://localhost:8000/ のページを更新し、「インストールは成功しました！おめでとうございます！」と表示されれば成功。Djangoサーバが停止していたら、起動する。


#### 参考
- 株式会社オープントーン，佐藤大輔，伊東直喜，上野啓二，『実装で学ぶフルスタックWeb開発 エンジニアの視野と知識を広げる「一気通貫」型ハンズオン』，翔泳社，2023．


