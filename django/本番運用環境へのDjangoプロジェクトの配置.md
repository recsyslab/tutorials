# 本番運用環境へのDjangoプロジェクトの配置.md

## サーバへの接続（クライアント側）
```bash
$ ssh conoha_rsl＊＊＊
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-52-generic x86_64)
...（略）...
rsl@＊:~$ 
```

## SSH接続の設定（サーバ側）

### SSH Keyの設定
```bash
rsl@＊:~$ ssh-keygen -t rsa -C "y＊＊＊＊＊＊@mail.ryukoku.ac.jp"
ssh-keygen -t rsa -C "y＊＊＊＊＊＊@mail.ryukoku.ac.jp"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_rsa): （デフォルトのままEnterキーを押す）
Enter passphrase (empty for no passphrase): 【パスフレーズ】
Enter same passphrase again: 【パスフレーズ】
Your identification has been saved in /home/rsl/.ssh/id_rsa
Your public key has been saved in /home/rsl/.ssh/id_rsa.pub
The key fingerprint is:
...（略）...
rsl@＊:~$ ls ~/.ssh/
rsl@＊:~$ cat ~/.ssh/id_rsa.pub
```

### 公開鍵の登録
1. [GitHub Dashboard](https://github.com/dashboard)の右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: `y＊＊＊＊＊＊@＊＊＊.＊＊＊.＊＊＊.＊＊＊`（`＊＊＊.＊＊＊.＊＊＊.＊＊＊`は接続元ののIPアドレス）
         - **Key**: `id_rsa.pub`の内容を貼り付ける。※`id_rsa`ではないので注意
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

### SSH接続の確認
下記コマンドで次のようなメッセージが表示されれれば接続成功。
```bash
rsl@＊:~$ ssh -T git@github.com
Enter passphrase for key '/home/rsl/.ssh/id_rsa': 【パスフレーズ】
Hi y＊＊＊＊＊＊! You've successfully authenticated, but GitHub does not provide shell access.
```

## リポジトリのclone（サーバ側）
```bash
rsl@＊:~$ cd
rsl@＊:~$ git clone git@github.com:recsyslab/rsl＊＊＊.git
rsl@＊:~$ ls ~/rsl＊＊＊/
```

## リポジトリのpull（サーバ側）
```bahs
rsl@＊:~$ cd ~/rsl＊＊＊/
rsl@＊:~$ git pull
```

## プロジェクト用の仮想環境の構築

### インストール済みパッケージリストの出力（クライアント側）
```bash
$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) $ pip freeze > venv_recsys_django_requirements.txt
$ scp ~/venv_recsys_django_requirements.txt conoha@rsl＊＊＊:/home/rsl/
```

### パッケージリストの読み込み（サーバ側）
```bash
rsl@＊:~$ cd
rsl@＊:~$ python3.11 -m venv ~/venv_recsys_django
rsl@＊:~$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) rsl@＊$ pip install --upgrade pip
(venv_recsys_django) rsl@＊$ pip install -r ~/venv_recsys_django_requirements.txt
(venv_recsys_django) rsl@＊$ pip freeze
(venv_recsys_django) rsl@＊$ pip install gunicorn
(venv_recsys_django) rsl@＊$ deactivate
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
【サーバのIPアドレス】$ echo -e "export ALLOWED_HOSTS=【サーバのIPアドレス】" >> ~/.profile
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
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ cp -r ~/rsl000_test/recsys_django/media/* /usr/share/nginx/html/media/
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

## データの登録
```bash
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ psql recsys_django -U rsl -c "\copy users from '/home/rsl/data/users.csv' with DELIMITER E'\t' CSV HEADER;"
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ psql recsys_django -U rsl -c "\copy items from '/home/rsl/data/items.csv' with DELIMITER E'\t' CSV HEADER;"
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ psql recsys_django -U rsl -c "\copy ratings from '/home/rsl/data/ratings.csv' with DELIMITER E'\t' CSV HEADER;"
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ psql recsys_django -U rsl -c "\copy reclist_popularity from '/home/rsl/data/reclist_popularity.csv' with DELIMITER E'\t' CSV HEADER;"
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ psql recsys_django -U rsl -c "\copy reclist_similarity from '/home/rsl/data/reclist_similarity.csv' with DELIMITER E'\t' CSV HEADER;"
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ psql recsys_django -U rsl -c "\copy reclist_itemcf from '/home/rsl/data/reclist_itemcf.csv' with DELIMITER E'\t' CSV HEADER;"
```

```pgsql
recsys_django=# SELECT setval('ratings_id_seq', (SELECT max(id) FROM ratings));
recsys_django=# SELECT setval('reclist_popularity_id_seq', (SELECT max(id) FROM reclist_popularity));
recsys_django=# SELECT setval('reclist_similarity_id_seq', (SELECT max(id) FROM reclist_similarity));
recsys_django=# SELECT setval('reclist_itemcf_id_seq', (SELECT max(id) FROM reclist_itemcf));
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
【サーバのIPアドレス】$ sudo nginx -t
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

## CSRF対策
開発環境ではPOSTできるのにデプロイ環境ではCSRF検証に失敗する。
以下のように設定を追記する。

`/etc/nginx/sites-available/【Djangoプロジェクト名】`
```bash
...（略）...
    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X_Forwarded-Proto $scheme;
        
        proxy_set_header X-Forwarded-Port $server_port;    # 追記
        proxy_set_header X-Forwarded-Host $host;           # 追記
        
        proxy_pass http://127.0.0.1:8000;
    }
...（略）...
```

以下を追記する。
`recsys_django/recsys_django/settings.py`
```py
...（略）...
DEPLOY = True
if DEPLOY:
    SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
    USE_X_FORWARDED_HOST = True
    USE_X_FORWARDED_PORT = True
```

```bash
【サーバのIPアドレス】$ sudo nginx -t
【サーバのIPアドレス】$ sudo systemctl reload nginx
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ pkill gunicorn
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ gunicorn --bind 127.0.0.1:8000 【Djangoプロジェクト名】.wsgi -D
```
#### 参考
- [runserverではPOSTできるのに、本番環境でCSRF validation エラーになる（解決） #Django - Qiita](https://qiita.com/gmasa/items/f136ddfd4fd36d7348d1)

## セキュリティ

```bash
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ python manage.py check --deploy
$ vi ~/recsys_django/recsys_django/settings.py`
```

以下を追記する。
`recsys_django/recsys_django/settings.py`
```py
...（略）...
DEPLOY = True
if DEPLOY:
    SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
    USE_X_FORWARDED_HOST = True
    USE_X_FORWARDED_PORT = True

    # 以下を追記
    #SECURE_HSTS_SECONDS = 60
    SECURE_HSTS_INCLUDE_SUBDOMAINS = True
    SECURE_CONTENT_TYPE_NOSNIFF = True
    SECURE_BROWSER_XSS_FILTER = True
    SECURE_SSL_REDIRECT = True
    SESSION_COOKIE_SECURE = True
    CSRF_COOKIE_SECURE = True
    X_FRAME_OPTIONS = "DENY"
    SECURE_HSTS_PRELOAD = True
```

```bash
(【Djangoプロジェクト名】) 【サーバのIPアドレス】$ python manage.py check --deploy
```

## ログの確認
```bash
$ less logs/django.log
```


