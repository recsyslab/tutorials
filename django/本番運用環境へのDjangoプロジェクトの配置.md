# 本番運用環境へのDjangoプロジェクトの配置.md

## サーバへの接続（クライアント側）
```bash
$ ssh conoha_rsl＊＊＊
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-52-generic x86_64)
...（略）...
rsl@＊:~$ 
```

## GitHubへのSSH接続の設定（サーバ側）

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

## GitHubリポジトリのclone（サーバ側）
```bash
rsl@＊:~$ cd
rsl@＊:~$ git clone git@github.com:recsyslab/rsl＊＊＊.git
rsl@＊:~$ ls ~/rsl＊＊＊/
```

## GitHubリポジトリのpull（サーバ側）
2回目以降、差分を反映させる場合は、下記コマンドを実行する。
```bahs
rsl@＊:~$ cd ~/rsl＊＊＊/
rsl@＊:~$ git pull
```

## venv_recsys_django仮想環境の構築

### インストール済みパッケージ一覧の出力（クライアント側）
```bash
$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) $ pip freeze > venv_recsys_django_requirements.txt
$ scp ~/venv_recsys_django_requirements.txt conoha@rsl＊＊＊:/home/rsl/
```

### パッケージのインストール（サーバ側）
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

## 本番運用環境用Djangoプロジェクト設定ファイルの編集
```bash
rsl@＊:~$ vi ~/rsl＊＊＊/recsys_django/recsys_django/settings.py
```

リスト1: `recsys_django/recsys_django/settings.py`
```py
...（略）...
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False                                      # 修正

ALLOWED_HOSTS = [os.environ.get('ALLOWED_HOSTS')]  # 修正
...（略）...
STATIC_URL = 'static/'
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)
STATIC_ROOT = '/usr/share/nginx/html/static/'    # 追記

MEDIA_URL = 'media/'                             # 追記
MEDIA_ROOT = '/usr/share/nginx/html/media/'      # 追記
...（略）...
# ロギング
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,

    # ロガーの設定
    'loggers': {
        # Djangoが利用するロガー
        'django': {
            'handlers': ['file'],
            'level': 'INFO',
        },
        # onlineアプリケーションが利用するロガー
        'online': {
            'handlers': ['file'],
            'level': 'INFO',
        },
    },

    # ハンドラの設定
    'handlers': {
        'file': {
            'level': 'INFO',
            'class': 'logging.handlers.TimedRotatingFileHandler',
            'filename': os.path.join(BASE_DIR, 'logs/django.log'),
            'formatter': 'prod',
            'when': 'D',        # ログローテーション（新しいファイルへの切り替え）間隔の単位（D=日）
            'interval': 1,      # ログローテーション間隔（1日単位）
            'backupCount': 7,   # 保存しておくログファイル数
        },
    },

    # フォーマッタの設定
    'formatters': {
        'prod': {
            'format': '\t'.join([
                '%(asctime)s',
                '[%(levelname)s]',
                '%(pathname)s(Line:%(lineno)d)',
                '%(message)s',
            ])
        },
    },
}
```

## ログ配置ディレクトリの作成
```bash
rsl@＊:~$ mkdir ~/rsl＊＊＊/recsys_django/logs/
```

## 環境変数の設定
```bash
rsl@＊$ less ~/.profile
rsl@＊$ echo -e '\n# Django用環境変数' >> ~/.profile
rsl@＊$ echo 'export DB_USER=rsl' >> ~/.profile
rsl@＊$ echo 'export DB_PASSWORD=【DBパスワード】' >> ~/.profile
rsl@＊$ echo 'export DJANGO_SETTINGS_MODULE=recsys_django.settings' >> ~/.profile
rsl@＊$ echo -e "export ALLOWED_HOSTS=rsl＊＊＊.recsyslab-ex.org" >> ~/.profile # rsl＊＊＊はRSL番号
rsl@＊$ less ~/.profile
rsl@＊$ diff ~/.profile-org ~/.profile
rsl@＊$ source ~/.profile
rsl@＊$ env
```

## 静的ファイルを配信ディレクトリに配置
```bash
rsl@＊$ sudo mkdir -p /usr/share/nginx/html/static/
rsl@＊$ sudo mkdir -p /usr/share/nginx/html/media/
rsl@＊$ sudo chown rsl /usr/share/nginx/html/static/
rsl@＊$ sudo chown rsl /usr/share/nginx/html/media/
rsl@＊$ ls -l /usr/share/nginx/html/
rsl@＊$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) rsl@＊$ cd ~/rsl＊＊＊/recsys_django/
(venv_recsys_django) rsl@＊$ python manage.py collectstatic
rsl@＊$ deactivate
rsl@＊$ cp -r ~/rsl＊＊＊/recsys_django/media/* /usr/share/nginx/html/media/
rsl@＊$ ls /usr/share/nginx/html/static/
rsl@＊$ ls /usr/share/nginx/html/media/
```

