# NginxとGunicornの起動

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

## Nginxの起動
```bash
rsl@＊$ sudo systemctl start nginx.service
rsl@＊$ systemctl status nginx.service
```

## Gunicornの起動
```bash
rsl@＊$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) rsl@＊$ cd ~/rsl＊＊＊/recsys_django/
(venv_recsys_django) rsl@＊$ gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
(venv_recsys_django) rsl@＊$ ps ax | grep gunicorn
 148964 ?        S      0:00 /home/rsl/venv_recsys_django/bin/python3.11 /home/rsl/venv_recsys_django/bin/gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
 148965 ?        S      0:00 /home/rsl/venv_recsys_django/bin/python3.11 /home/rsl/venv_recsys_django/bin/gunicorn --bind 127.0.0.1:8000 recsys_django.wsgi -D
 148976 pts/0    S+     0:00 grep --color=auto gunicorn
```

## Webサーバの動作確認
下記にアクセスし、recsys-djangoのトップページが表示されれば、正常に稼働している。
- ※`https://rsl＊＊＊.recsyslab-ex.org/`（`rsl＊＊＊`はRSL番号）
- ※ConoHaコントロールパネルから接続許可ポートが`Web (20/21/80/443)`にチェックが入っていることを確認する。

## Gunicornの停止
Gunicornを停止する場合は下記コマンドを実行する。Nginxの設定などを編集した場合は、Gunicornを一旦停止した後で、Gunicornを起動する。
```bash
(venv_recsys_django) rsl@＊$ pkill gunicorn
(venv_recsys_django) rsl@＊$ ps ax | grep gunicorn
 149114 pts/0    S+     0:00 grep --color=auto gunicorn
```

## ログの確認
サーバにエラーが発生した場合などは、下記コマンドでログを確認する。
```bash
rsl@＊$ less ~/rsl＊＊＊/recsys_django/logs/django.log
```

#### 参考
1. [DjangoでgunicornとNginxを使う（Nginxその3） | Snow Tree in June](https://snowtree-injune.com/2020/11/07/nginx-part3-dj015/)
