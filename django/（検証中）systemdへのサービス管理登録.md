# systemdへのサービス管理登録

## serviceタイプのUnit定義ファイルの作成
```bash
rsl@＊$ ls /etc/systemd/system/
rsl@＊$ sudo vi /etc/systemd/system/recsys_django.service
```

リスト1: `/etc/systemd/system/recsys_django.service`
```bash
[Unit]
Description=gunicorn daemon (recsys_django)
Requires=recsys_django.socket
After=network.target

[Service]
User=rsl
Group=www-data
WorkingDirectory=/home/rsl/rsl＊＊＊/recsys_django/
ExecStart=/home/rsl/venv_recsys_django/bin/gunicorn --workers 3 --bind unix:/run/gunicorn/recsys_django.sock recsys_django.wsgi:application

[Install]
WantedBy=multi-user.target
```

```bash
rsl@＊$ less /etc/systemd/system/recsys_django.service
```

## socketタイプのUnit定義ファイルの作成
```bash
rsl@＊$ sudo vi /etc/systemd/system/recsys_django.socket
```

リスト2: `/etc/systemd/system/recsys_django.socket`
```bash
[Unit]
Description=gunicorn socket (recsys_django)

[Socket]
ListenStream=/run/gunicorn/recsys_django.sock

[Install]
WantedBy=sockets.target
```

```bash
rsl@＊$ less /etc/systemd/system/recsys_django.socket
```

## systemdへのサービス管理登録
```bash
rsl@＊$ sudo systemctl enable recsys_django.socket
Created symlink /etc/systemd/system/sockets.target.wants/recsys_django.socket → /etc/systemd/system/recsys_django.socket.
rsl@＊$ ls /etc/systemd/system/sockets.target.wants/
rsl@＊$ sudo systemctl enable recsys_django.service
Created symlink /etc/systemd/system/multi-user.target.wants/recsys_django.service → /etc/systemd/system/recsys_django.service.
rsl@＊$ ls /etc/systemd/system/multi-user.target.wants/
```

## サービスの起動
```bash
rsl@＊$ sudo systemctl start recsys_django
# /run/gunicorn/recsys_django.sockが自動作成される。
rsl@＊$ ls -l /run/gunicorn/
rsl@＊$ systemctl is-enabled recsys_django.socket
rsl@＊$ sudo systemctl status recsys_django.socket
rsl@＊$ systemctl is-enabled recsys_django
rsl@＊$ sudo systemctl status recsys_django
```

## Unit定義ファイルの再読込み
Unit定義ファイルを修正した場合は、下記コマンドを実行して、定義を再読込みしたうえで、サービスを再起動する。
```bash
rsl@＊$ sudo systemctl daemon-reload
rsl@＊$ sudo systemctl restart recsys_django
```

## Gunicornの停止
Gunicornを停止する場合は下記コマンドを実行する。Nginxの設定などを編集した場合は、Gunicornを一旦停止した後で、Gunicornを起動する。
```bash
(venv_recsys_django) rsl@＊$ pkill gunicorn
(venv_recsys_django) rsl@＊$ ps ax | grep gunicorn
 149114 pts/0    S+     0:00 grep --color=auto gunicorn
```

## Nginxの設定の修正
```bash
rsl@＊$ sudo vi /etc/nginx/sites-available/recsys_django
```

リスト3: `/etc/nginx/sites-available/recsys_django`
```bash
server {
...（略）...
    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X_Forwarded-Proto $scheme;
        
#        proxy_pass http://127.0.0.1:8000;                          # コメントアウト
        proxy_pass http://unix:/run/gunicorn/recsys_django.sock;    # 追記
    }
...（略）...
}
```

```bash
rsl@＊$ less /etc/nginx/sites-available/recsys_django
rsl@＊$ sudo nginx -t
rsl@＊$ sudo systemctl reload nginx
```

## サービスの再起動
```bash
rsl@＊$ sudo systemctl restart recsys_django
rsl@＊$ ps ax | grep gunicorn
```

## Webサーバの動作確認
下記にアクセスし、recsys-djangoのトップページが表示されれば、正常に稼働している。
- ※`https://rsl＊＊＊.recsyslab-ex.org/`（`rsl＊＊＊`はRSL番号）
- ※2023年11月3日現在、不具合あり：上記にアクセスすると「400 Bad Request」と表示される。


#### 参考
1. 現場で使える Django の教科書《実践編》 # 第7章 デプロイ
1. [Gunicorn設定 - 株式会社日本ビューシステム](https://view-s.co.jp/product/webapp/wsgi/)
1. [Gunicorn用のSystemdソケットとサービスファイルの作成 #Django - Qiita](https://qiita.com/mono11/items/a0a0996f80d86bd7a68c)
