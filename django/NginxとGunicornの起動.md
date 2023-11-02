
## Nginxの設定
```bash
rsl@＊$ ls /etc/nginx/sites-available/
rsl@＊$ less /etc/nginx/sites-available/default
rsl@＊$ sudo vi /etc/nginx/sites-available/recsys_django
```

リスト2: `/etc/nginx/sites-available/recsys_django`
- `/etc/nginx/sites-available/default`を参考に下記のように作成する。
- ※`rsl＊＊＊`はRSL番号
```bash
server {
    server_name rsl＊＊＊.recsyslab-ex.org; # managed by Certbot
    
    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/rsl＊＊＊.recsyslab-ex.org/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/rsl＊＊＊.recsyslab-ex.org/privkey.pem; # managed by Certbot
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
rsl@＊$ ls /etc/nginx/sites-available/
rsl@＊$ less /etc/nginx/sites-available/recsys_django
rsl@＊$ ls -al /etc/nginx/sites-enabled/
rsl@＊$ sudo ln -s /etc/nginx/sites-available/recsys_django /etc/nginx/sites-enabled/
rsl@＊$ sudo unlink /etc/nginx/sites-enabled/default
rsl@＊$ ls -al /etc/nginx/sites-enabled/
rsl@＊$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
rsl@＊$ sudo systemctl reload nginx
```

## NginxとGunicornの起動

### Nginxの起動
```bash
rsl@＊$ sudo systemctl start nginx.service
rsl@＊$ systemctl status nginx.service
```

### Gunicornの起動
```bash
rsl@＊$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) rsl@＊$ cd ~/rsl＊＊＊/recsys_django/
(venv_recsys_django) rsl@＊$ gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
(venv_recsys_django) rsl@＊$ ps ax | grep gunicorn
 148964 ?        S      0:00 /home/rsl/venv_recsys_django/bin/python3.11 /home/rsl/venv_recsys_django/bin/gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
 148965 ?        S      0:00 /home/rsl/venv_recsys_django/bin/python3.11 /home/rsl/venv_recsys_django/bin/gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
 148976 pts/0    S+     0:00 grep --color=auto gunicorn
```

### Webサーバの動作確認
下記にアクセスし、recsys-djangoのトップページが表示されれば、正常に稼働している。
- ※`https://rsl＊＊＊.recsyslab-ex.org/`（`rsl＊＊＊`はRSL番号）
- ※ConoHaコントロールパネルから接続許可ポートが`Web (20/21/80/443)`にチェックが入っていることを確認する。

### Gunicornの停止
Gunicornを停止する場合は下記コマンドを実行する。Nginxの設定などを編集した場合は、Gunicornを一旦停止した後で、Gunicornを起動する。
```bash
(venv_recsys_django) rsl@＊$ pkill gunicorn
(venv_recsys_django) rsl@＊$ ps ax | grep gunicorn
 149114 pts/0    S+     0:00 grep --color=auto gunicorn
```

### ログの確認
サーバにエラーが発生した場合などは、下記コマンドでログを確認する。
```bash
rsl@＊$ less ~/rsl＊＊＊/recsys_django/logs/django.log
```

## CSRF対策
開発環境ではPOSTできるのにデプロイ環境ではCSRF検証に失敗する場合、以下のようにNginxの設定を追記する。

```bash
rsl@＊$ sudo vi /etc/nginx/sites-available/recsys_django
```

リスト3: `/etc/nginx/sites-available/recsys_django`
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

```bash
rsl@＊$ less /etc/nginx/sites-available/recsys_django
```

```bash
rsl@＊:~$ vi ~/rsl＊＊＊/recsys_django/recsys_django/settings.py
```

リスト4: `recsys_django/recsys_django/settings.py`
```py
...（略）...
DEPLOY = True
if DEPLOY:
    SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
    USE_X_FORWARDED_HOST = True
    USE_X_FORWARDED_PORT = True
```

```bash
rsl@＊$ sudo nginx -t
rsl@＊$ sudo systemctl reload nginx
rsl@＊$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) rsl@＊$ pkill gunicorn
(venv_recsys_django) rsl@＊$ gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
```

正常にPOSTできるか確認する。

#### 参考
- [runserverではPOSTできるのに、本番環境でCSRF validation エラーになる（解決） #Django - Qiita](https://qiita.com/gmasa/items/f136ddfd4fd36d7348d1)

## セキュリティ

```bash
(venv_recsys_django) rsl@＊$ python manage.py check --deploy
(venv_recsys_django) rsl@＊$ vi ~/rsl＊＊＊/recsys_django/recsys_django/settings.py
```

以下を追記する。
リスト5: `recsys_django/recsys_django/settings.py`
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
(venv_recsys_django) rsl@＊$ python manage.py check --deploy
```

#### 参考
1. 動かして学ぶ！Python Django開発入門 第2版 # Chapter 13 独自ドメイン化とセキュリティ対策

#### 参考
1. 動かして学ぶ！Python Django開発入門 第2版 # Chapter 12 Djangoとクラウドを連携して本番運用を行う
1. 現場で使える Django の教科書《実践編》 # 第7章 デプロイ
