# 本番運用環境へのDjangoプロジェクトの配置.md

## GitHubリポジトリへのpush
```bash
$ cd ~/【リポジトリ名】/
$ git add *
$ git status
$ git commit -m "（任意のメッセージ）"
$ git push
```

## サーバへの接続
```bash
$ ssh 【サーバにアクセスするユーザ名】@【サーバのIPアドレス】
【サーバのIPアドレス】$
```

## SSH Keyの設定
```bash
【サーバのIPアドレス】$ ssh-keygen -t rsa -C "【メールアドレス】"
【サーバのIPアドレス】$ ls ~/.ssh/
【サーバのIPアドレス】$ cat ~/.ssh/id_rsa.pub
```

## 公開鍵の登録
1. GitHubの右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: 【任意の鍵の名前】
         - **Key**: `id_rsa.pub`の内容を貼り付ける。
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

## リポジトリのclone
```bash
【サーバのIPアドレス】$ cd
【サーバのIPアドレス】$ mkdir -p 【リポジトリ名】/
【サーバのIPアドレス】$ git clone git@github.com:recsyslab/【リポジトリ名】.git
【サーバのIPアドレス】$ cd 【リポジトリ名】/
【サーバのIPアドレス】$ ls
```

## リポジトリのpull
```bahs
【サーバのIPアドレス】$ cd 【リポジトリ名】/
【サーバのIPアドレス】$ git pull
```

## プロジェクト用の仮想環境の構築
```bash
$ scp ~/venv/rsl-django_requirements.txt 【サーバにアクセスするユーザ名】@【サーバのIPアドレス】:/home/rsl/venv/
```

```bash
【サーバのIPアドレス】$ cd ~/venv/
【サーバのIPアドレス】$ python3.11 -m venv 【Djangoプロジェクト名】
【サーバのIPアドレス】$ source ~/venv/【Djangoプロジェクト名】/bin/activate
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ pip install --upgrade pip
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ pip install -r ~/venv/rsl-django_requirements.txt
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ pip freeze
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ pip install gunicorn
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ deactivate
```

## ログは一ディレクトリの作成
```bash
【サーバのIPアドレス】$ mkdir ~/【リポジトリ名】/【Djangoプロジェクト名】/logs/
```

## 環境変数の設定
```bash
【サーバのIPアドレス】$ less ~/.profile
【サーバのIPアドレス】$ echo -e '\n# Django用環境変数' >> ~/.profile
【サーバのIPアドレス】$ echo 'export DB_USER=rsl' >> ~/.profile
【サーバのIPアドレス】$ echo 'export DB_PASSWORD=【DBパスワード】' >> ~/.profile
【サーバのIPアドレス】$ echo 'export DJANGO_SETTINGS_MODULE=【Djangoプロジェクト名】.settings' >> ~/.profile
【サーバのIPアドレス】$ echo -e "export ALLOWED_HOSTS=\"['【サーバのIPアドレス】', '【サーバのIPアドレス】', 'localhost', '127.0.0.1']\"" >> ~/.profile
【サーバのIPアドレス】$ less ~/.profile
【サーバのIPアドレス】$ diff ~/.profile-org ~/.profile
【サーバのIPアドレス】$ source ~/.profile
【サーバのIPアドレス】$ env
```

## 静的ファイルを配信ディレクトリに配置
```bash
【サーバのIPアドレス】$ sudo mkdir -p /usr/share/nginx/html/static/
【サーバのIPアドレス】$ sudo mkdir -p /usr/share/nginx/html/media/
【サーバのIPアドレス】$ sudo chown rsl /usr/share/nginx/html/static/
【サーバのIPアドレス】$ sudo chown rsl /usr/share/nginx/html/media/
【サーバのIPアドレス】$ source ~/venv/【Djangoプロジェクト名】/bin/activate
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ cd ~/【リポジトリ名】/【Djangoプロジェクト名】/
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ python manage.py collectstatic
```

## データベース環境の構築（前もってやっておく）
https://recsyslab.github.io/recsys-django/
06 データベース環境の構築
11 ユーザ、アイテム、評価値テーブルの設計とデータの登録
12 推薦リストテーブルの設計とデータの登録

## マイグレーション
```bash
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ python manage.py migrate
```

## Nginxの設定
Nginxのインストール、自動起動などの初期設定は先に行っておく。
```bash
【サーバのIPアドレス】$ sudo vi /etc/nginx/sites-available/【Djangoプロジェクト名】
```

`/etc/nginx/sites-available/【Djangoプロジェクト名】`
```
server {
    server_name recsyslab-ex.org www.recsyslab-ex.org; # managed by Certbot
    
    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/recsyslab-ex.org/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/recsyslab-ex.org/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    
    root /usr/share/nginx/html;
    
    location /static {
        alias /usr/share/nginx/html/static;
    }
    
    location /media {
        alias /usr/share/nginx/html/media;
    }
    
    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X_Forwarded-Proto $scheme;
        
        proxy_pass http://127.0.0.1:8000;
    }
    
    error_page 404 /404.html;
    location = /40x.html {
    }
    
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
    
    # index.html等はファイル名の指定なしで実行
    index index.html index.htm index.nginx-debian.html;
}
```

```bash
【サーバのIPアドレス】$ ls -al /etc/nginx/sites-enabled/
【サーバのIPアドレス】$ sudo ln -s /etc/nginx/sites-available/【Djangoプロジェクト名】 /etc/nginx/sites-enabled/
【サーバのIPアドレス】$ sudo unlink /etc/nginx/sites-enabled/default
【サーバのIPアドレス】$ ls -al /etc/nginx/sites-enabled/
【サーバのIPアドレス】$ sudo systemctl reload nginx
```

## NginxとGunicornの起動
```bash
【サーバのIPアドレス】$ sudo systemctl start nginx.service
【サーバのIPアドレス】$ systemctl status nginx.service
```

```bash
【サーバのIPアドレス】$ source ~/venv/【Djangoプロジェクト名】/bin/activate
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ cd ~/【リポジトリ名】/【Djangoプロジェクト名】/
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ gunicorn --bind 127.0.0.1:8000 【Djangoプロジェクト名】.wsgi -D
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ ps ax | grep gunicorn
```
