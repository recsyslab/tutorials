# CSRF対策
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
