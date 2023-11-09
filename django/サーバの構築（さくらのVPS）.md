# サーバの構築（さくらのVPS）

## さくらのVPSコントロールパネルへのアクセス
1. 下記からさくらのVPSコントロールパネルを開く。
   - さくらのVPS - コントロールパネル](https://secure.sakura.ad.jp/vps/)

## SSH Keyの作成

### ssh-keygenから（クライアント側）
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_rsa): /home/rsl/.ssh/sakura-rsl＊＊＊.pem.key（rsl＊＊＊はRSL番号）
Enter passphrase (empty for no passphrase): 【パスフレーズ】
Enter same passphrase again: 【パスフレーズ】
Your identification has been saved in /home/rsl/.ssh/sakura-rsl＊＊＊.pem.key
Your public key has been saved in /home/rsl/.ssh/sakura-rsl＊＊＊.pem.key.pub
The key fingerprint is:
SHA256:＊＊＊＊＊＊＊＊ rsl@rsl-local
The key's randomart image is:
+---[RSA 3072]----+
...（略）...
+----[SHA256]-----+
$ ls ~/.ssh/
$ cat ~/.ssh/sakura-rsl＊＊＊.pem.key.pub
```

1. さくらのVPSコントロールパネルから、**SSHキー管理**を開く。
   1. **SSHキー登録**ボタンをクリックする。
   2. 下記を設定する。
      - **名前**: `key-sakura-rsl＊＊＊`（`＊＊＊`はRSL番号）
      - **公開鍵**: `sakura-rsl＊＊＊.pem.key.pub`の内容を貼り付ける。※`sakura-rsl＊＊＊.pem.key`ではないので注意
   3. **SSHキーを登録**ボタンをクリックする。

## サーバの追加
1. さくらのVPSコントロールパネルから、**新規追加▼**ボタンをクリックする。
