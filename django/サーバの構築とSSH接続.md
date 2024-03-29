# サーバの構築とSSH接続

## ConoHaコントロールパネルへのアクセス
1. 下記からConoHaコントロールパネルを開く。
   - [ConoHaコントロールパネル](https://manage.conoha.jp/Dashboard/)

## SSH Keyの作成

### ConoHaコントロールパネルから
1. ConoHaコントロールパネルから、**セキュリティ > SSH Key**を開く。
   1. **+パブリックキー**ボタンをクリックする。
   2. 下記を設定する。
      - **リージョン**: `東京`
      - **登録方法**: `自動作成`
      - **ネームタグ**: `key-conoha-rsl＊＊＊`（`＊＊＊`はRSL番号）
   3. **保存**ボタンをクリックする。
   4. **ダウンロード**ボタンをクリックし、プライベートキーをダウンロードする。
   5. ダウンロードしたプライベートキーのファイル名を`conoha-rsl＊＊＊.pem.key`に変更する。

### ssh-keygenから（クライアント側）
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_rsa): /home/rsl/.ssh/conoha-rsl＊＊＊.pem.key（rsl＊＊＊はRSL番号）
Enter passphrase (empty for no passphrase): 【パスフレーズ】
Enter same passphrase again: 【パスフレーズ】
Your identification has been saved in /home/rsl/.ssh/conoha-rsl＊＊＊.pem.key
Your public key has been saved in /home/rsl/.ssh/conoha-rsl＊＊＊.pem.key.pub
The key fingerprint is:
SHA256:＊＊＊＊＊＊＊＊ rsl@rsl-local
The key's randomart image is:
+---[RSA 3072]----+
...（略）...
+----[SHA256]-----+
$ ls ~/.ssh/
$ cat ~/.ssh/conoha-rsl＊＊＊.pem.key.pub
```

1. ConoHaコントロールパネルから、**セキュリティ > SSH Key**を開く。
   1. **+パブリックキー**ボタンをクリックする。
   2. 下記を設定する。
      - **リージョン**: `東京`
      - **登録方法**: `インポート`
      - **ネームタグ**: `key-conoha-rsl＊＊＊`（`＊＊＊`はRSL番号）
      - **パブリックキー**: `conoha-rsl＊＊＊.pem.key.pub`の内容を貼り付ける。※`conoha-rsl＊＊＊.pem.key`ではないので注意
   3. **保存**ボタンをクリックする。

#### 参考
1. [SSH Keyを登録する｜ConoHa VPSサポート](https://support.conoha.jp/v/registsshkey/)

## サーバの追加

### 購入済みサーバの場合
1. ConoHaコントロールパネルから、**サーバー追加**を開く。
   1. **VPS**タブを開く。
   2. 下記を設定する。
      - **料金タイプ**: `VPS割引きっぷSSLセット`
      - **利用するきっぷ**: `購入済み`
      - **プラン**: `メモリ 8GB / 2023-07-22 ~ 2024-07-22`
      - **イメージタイプ > OS**: `Ubuntu 22.04 (64bit)`
      - **root パスワード**: 【パスワード】
      - **ネームタグ**: `rsl＊＊＊`（`＊＊＊`はRSL番号）
      - **オプション**
        - **自動バックアップ**: `無効`
        - **追加ディスク**: `使用しない`
        - **接続許可ポート IPv4**: `SSH (22)`、`Web (20/21/80/443)`
        - **接続許可ポート IPv6**: `全て拒否`
        - **SSH Key**: `登録済みキー`
          - **パブリックキー**: `key-conoha-rsl＊＊＊`（**SSH Keyの作成**で作成したキー）
        - **スタートアップスクリプト**: `使用しない`
   3. **追加**ボタンをクリックする。

### 時間課金で新規に利用する場合
1. ConoHaコントロールパネルから、**サーバー追加**を開く。
   1. **VPS**タブを開く。
   2. 下記を設定する。
      - **料金タイプ**: `時間課金`
      - **プラン**: `メモリ 1GB / CPU 2Core / SSD 100GB`
      - **イメージタイプ > OS**: `Ubuntu 22.04 (64bit)`
      - **root パスワード**: 【パスワード】
      - **ネームタグ**: `rsl＊＊＊`（`＊＊＊`はRSL番号）
      - **オプション**
        - **自動バックアップ**: `無効`
        - **追加ディスク**: `使用しない`
        - **接続許可ポート IPv4**: `SSH (22)`、`Web (20/21/80/443)`
        - **接続許可ポート IPv6**: `全て拒否`
        - **SSH Key**: `登録済みキー`
          - **パブリックキー**: `key-conoha-rsl＊＊＊`（**SSH Keyの作成**で作成したキー）
        - **スタートアップスクリプト**: `使用しない`
   3. **追加**ボタンをクリックする。

## サーバの起動
1. ConoHaコントロールパネルから、**サーバー**を開く。
   1. **サーバーリスト**から起動したいサーバのネームタグをクリックする。
      1. **コンソール**ボタンをクリックする。
      2. 端末が開くので、下記のとおりrootでログインする。

- ※`＊＊＊-＊＊＊-＊＊＊-＊＊＊`はサーバのIPアドレス
- ※`root@＊:~#`はサーバ側の`root`ユーザ、`rsl@＊:~$`はサーバ側の`rsl`ユーザ、`$`はクライアント側のユーザのプロンプトをそれぞれ表す。

```bash
Ubuntu 22.04.1 LTS ＊＊＊-＊＊＊-＊＊＊-＊＊＊ tty1

＊＊＊-＊＊＊-＊＊＊-＊＊＊ login: root
Password: 【パスワード】
root@＊:~#
```

## rslユーザの追加
```bash
root@＊:~# sudo adduser rsl
root@＊:~# sudo gpasswd -a rsl sudo
root@＊:~# cat /etc/passwd
root@＊:~# cat /etc/group
root@＊:~# groups rsl
root@＊:~# exit
```

ConoHaコントロールパネルの**コンソール**から作成したユーザでログインする。
```bash
Ubuntu 22.04.1 LTS ＊＊＊-＊＊＊-＊＊＊-＊＊＊ tty1

＊＊＊-＊＊＊-＊＊＊-＊＊＊ login: rsl
Password: 【パスワード】
rsl@＊:~$
```

## SSH接続の設定

### 設定ファイルの準備（サーバ側）
```bahs
rsl@＊:~$ ls /etc/ssh/
rsl@＊:~$ less /etc/ssh/sshd_config
rsl@＊:~$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config-org
rsl@＊:~$ ls /etc/ssh/
rsl@＊:~$ mkdir ~/.ssh/
```

### 公開鍵の設置（サーバ側）
1. ConoHaコントロールパネルから、**セキュリティ > SSH Key**を開く。
   1. **SSH Key**リストから`key-conoha-rsl＊＊＊`をクリックする。
   2. **パブリックキー**の内容をメモする。
2. ConoHaコントロールパネルの**コンソール**から下記を実行する。
```bash
rsl@＊:~$ cat << EOF > temp.txt
```
3. **コンソール**の**テキスト送信**をクリックし、メモした**パブリックキー**の内容を貼り付けて送信する。
   - ※一度にすべて送信しようとすると、送信漏れが発生する。そのため（80文字程度など）何回かに分割して送信する。
   - ※1字でも誤りがあると、SSH接続できないので注意。
4. すべて送信したら、改行して`EOF`と書いてファイルに出力する。

```bash
rsl@＊:~$ tr -d '\n' < temp.txt >> ~/.ssh/authorized_keys
rsl@＊:~$ chmod 700 ~/.ssh/
rsl@＊:~$ chmod 600 ~/.ssh/authorized_keys
rsl@＊:~$ ls -l
rsl@＊:~$ ls -l ~/.ssh/
rsl@＊:~$ less ~/.ssh/authorized_keys
rsl@＊:~$ rm -f temp.txt
```

### SSHの設定（サーバ側）
```bash
rsl@＊:~$ less /etc/ssh/sshd_config
rsl@＊:~$ sudo vi /etc/ssh/sshd_config
```

`sshd_config`の下記のように書き換える。

`/etc/ssh/sshd_config`
```bash
...（略）...
PermitRootLogin no
...（略）...
PasswordAuthentication no
...（略）...
```

```bash
rsl@＊:~$ less /etc/ssh/sshd_config
rsl@＊:~$ diff /etc/ssh/sshd_config-org /etc/ssh/sshd_config
rsl@＊:~$ sudo systemctl reload ssh
rsl@＊:~$ systemctl status ssh
rsl@＊:~$ systemctl list-unit-files --type=service | grep ssh
```

#### 参考
1. [一般ユーザーで公開鍵認証を使用してSSHログインする｜ConoHa VPSサポート](https://support.conoha.jp/v/addusersshkey/)
2. [ConoHa VPSの秘密鍵(pem)を無くしてハマった](https://zenn.dev/hasegit/articles/a4db90b3b95cb7)

### ファイアウォールの設定（サーバ側）
```bash
rsl@＊:~$ sudo ufw status
rsl@＊:~$ sudo ufw default deny
rsl@＊:~$ sudo ufw app list
rsl@＊:~$ sudo ufw app info OpenSSH
rsl@＊:~$ sudo ufw allow OpenSSH
rsl@＊:~$ sudo ufw status
rsl@＊:~$ sudo ufw reload
```

### SSHの設定（クライアント側）
```bash
$ sudo apt install ssh
$ mkdir ~/.ssh/
```

**SSH Keyの作成**でダウンロードしたプライベートキー`conoha-rsl＊＊＊.pem.key`を`~/.ssh/に置く。

```bash
$ vi ~/.ssh/config
```

`~/.ssh/config`
```bash
Host conoha_rsl＊＊＊（＊＊＊はRSL番号）
  HostName ＊＊＊.＊＊＊.＊＊＊.＊＊＊（＊＊＊.＊＊＊.＊＊＊.＊＊＊はサーバのIPアドレス）
  port 22
  User rsl
  IdentityFile ~/.ssh/conoha-rsl＊＊＊.pem.key（SSH Keyの作成で作成したキー）
```

```bash
$ chmod 700 ~/.ssh/
$ chmod 600 ~/.ssh/config
$ chmod 400 ~/.ssh/conoha-rsl＊＊＊.pem.key
$ ls -l
$ ls -l ~/.ssh/
```

### SSH接続の確認（クライアント側）

下記コマンドでサーバにSSH接続できれば接続成功。
- ※ConoHaコントロールパネルから接続許可ポートが`SSH (22)`にチェックが入っていることを確認する。
```bash
$ ssh conoha_rsl＊＊＊
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-52-generic x86_64)
...（略）...
rsl@＊:~$ 
```

#### 参考
1. [ConoHa VPS 公開鍵認証でSSH接続 #Conoha - Qiita](https://qiita.com/kaoru0404/items/9c5ff6d45462e9d06133)
