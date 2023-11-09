# サーバの構築（さくらのVPS）

## さくらのVPSコントロールパネルへのアクセス
1. 下記からさくらのVPSコントロールパネルを開く。
   - さくらのVPS - コントロールパネル](https://secure.sakura.ad.jp/vps/)

## SSH Keyの作成

### ssh-keygenから（クライアント側）
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_rsa): /home/rsl/.ssh/sakura-rsl＊＊＊.pem.key ※rsl＊＊＊はRSL番号
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
      - **名前**: `key-sakura-rsl＊＊＊` ※`rsl＊＊＊`はRSL番号
      - **公開鍵**: `sakura-rsl＊＊＊.pem.key.pub`の内容を貼り付ける。※`sakura-rsl＊＊＊.pem.key`ではないので注意
   3. **SSHキーを登録**ボタンをクリックする。

## サーバの追加
1. さくらのVPSコントロールパネルの右上の**新規追加 - VPS**を開く。
   1. 下記を設定する。
      - **OS**: `Ubuntu`
      - **バージョン**: `22.04 amd64`
      - **サーバーのプラン**:
        - `12ヶ月一括`
        - **1Gプラン: CPU 仮想2Core, メモリ 1GB, SSD 50GB**: `チェック`
        - **ストレージを大容量 SSD 100GB にアップグレードする**: `チェック`
        - **リージョン**: `大阪第3`
        - **購入台数**: `1`
      - **サーバーの管理ユーザー情報**
        - **管理ユーザー名**: `ubuntu`（デフォルト）
        - **管理ユーザーのパスワード**: 【パスワード】
        - **SSHキー設定**: `登録済みの公開鍵を使ってインストールする`
        - **key-sakura-rsl＊＊＊**（**SSH Keyの作成**で作成したキー）: `チェック`
        - **パスワードを利用したログインを許可する**: `非チェック`
      - **サーバーに関する設定**
        - **スタートアップスクリプト**: （空欄）
        - **サーバーの名前**: `rsl＊＊＊` ※`rsl＊＊＊`はRSL番号
   2. **お支払い方法選択へ**ボタンをクリックする。
   3. 画面にしたがって支払処理を進める。
   4. 「お申し込みが完了しました」画面が表示されれば完了。
