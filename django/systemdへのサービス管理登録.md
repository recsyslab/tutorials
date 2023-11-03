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
ExecStart=/home/rsl/venv_recsys_django/bin/gunicorn --workers 3 --bind /home/rsl/rsl＊＊＊/recsys_django/recsys_django.sock recsys_django.wsgi:application

[Install]
WantedBy=multi-user.target
```
