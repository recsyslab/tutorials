# nginxのインストール

## nginxのインストール
```bash
rsl@＊:~$ sudo apt update
rsl@＊:~$ sudo apt install nginx
```

## nginxの起動
```bash
rsl@＊:~$ systemctl is-enabled nginx
# 「enabled」と表示されれば、nginxの自動起動が有効になっている。
rsl@＊:~$ systemctl status nginx
# 「Active: active (running)」と表示されれば、nginxは稼働している。
```

## ファイアウォールの設定
```bash
rsl@＊:~$ sudo ufw status
rsl@＊:~$ sudo ufw app list
rsl@＊:~$ sudo ufw app info 'Nginx Full'
rsl@＊:~$ sudo ufw allow 'Nginx Full'
rsl@＊:~$ sudo ufw status
rsl@＊:~$ sudo ufw reload
```

## Webサーバの動作確認

下記にアクセスし、「Welcome to nginx!」と表示されれば、正常に稼働している。
- ※`http://＊＊＊.＊＊＊.＊＊＊.＊＊＊/`（`＊＊＊.＊＊＊.＊＊＊.＊＊＊`はサーバのIPアドレス）
- ※ConoHaコントロールパネルから接続許可ポートが`Web (20/21/80/443)`にチェックが入っていることを確認する。

#### 参考
1. [nginxでWebサーバーを構築する（Ubuntu編）- マルチドメイン・HTTPS化の設定も解説 - アナグマのモノローグ](https://monologu.com/nginx-ubuntu/)



