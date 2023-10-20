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
【サーバのホスト】$
```

## SSH Keyの設定
```bash
【サーバのホスト】$ ssh-keygen -t rsa -C "【メールアドレス】"
【サーバのホスト】$ ls ~/.ssh/
【サーバのホスト】$ cat ~/.ssh/id_rsa.pub
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
【サーバのホスト】$ cd
【サーバのホスト】$ mkdir -p 【リポジトリ名】/
【サーバのホスト】$ git clone git@github.com:recsyslab/【リポジトリ名】.git
【サーバのホスト】$ cd 【リポジトリ名】/
【サーバのホスト】$ ls
```

## リポジトリのpull
```bahs
【サーバのホスト】$ cd 【リポジトリ名】/
【サーバのホスト】$ git pull
```

## プロジェクト用の仮想環境の構築
```bash
$ scp ~/venv/rsl-django_requirements.txt 【サーバにアクセスするユーザ名】@【サーバのIPアドレス】:/home/rsl/venv/
```

```bash
【サーバのホスト】$ cd ~/venv/
【サーバのホスト】$ python3.11 -m venv 【Djangoプロジェクト名】
【サーバのホスト】$ source ~/venv/【Djangoプロジェクト名】/bin/activate
(【Djangoプロジェクト名】) 【サーバのホスト】$ pip install --upgrade pip
(【Djangoプロジェクト名】) 【サーバのホスト】$ pip install -r ~/venv/rsl-django_requirements.txt
(【Djangoプロジェクト名】) 【サーバのホスト】$ pip freeze
(【Djangoプロジェクト名】) 【サーバのホスト】$ pip install gunicorn
(【Djangoプロジェクト名】) 【サーバのホスト】$ deactivate
```

## ログは一ディレクトリの作成
```bash
【サーバのホスト】$ mkdir ~/【リポジトリ名】/【Djangoプロジェクト名】/logs/
```

