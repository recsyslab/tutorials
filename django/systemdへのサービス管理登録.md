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
ExecStart=/home/rsl/venv_recsys_django/bin/gunicorn --workers 3 --bind /run/gunicorn/recsys_django.sock recsys_django.wsgi:application

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
