# サーバーの構築

## ConoHaコントロールパネルへのアクセス
1. 下記からConoHaコントロールパネルを開く。
   - [ConoHaコントロールパネル](https://manage.conoha.jp/Dashboard/)

## SSH Keyの作成
1. **セキュリティ > SSH Key**をクリックする。
2. **+パブリックキー**ボタンをクリックする。
3. 下記を設定する。
   - **リージョン**: `東京`
   - **登録方法**: `自動作成`
   - **ネームタグ**: `key-conoha-rsl＊＊＊`（RSL番号）
4. **保存**ボタンをクリックする。
5. **ダウンロード**ボタンをクリックし、プライベートキーをダウンロードする。
6. ダウンロードしたプライベートキーのファイル名を`key-conoha-rsl＊＊＊.pem`に変更する。

#### 参考
1. [SSH Keyを登録する｜ConoHa VPSサポート](https://support.conoha.jp/v/registsshkey/)

## サーバーの追加

### 購入済みサーバーの場合
1. **サーバー追加**をクリックする。
2. **VPS**タブを開く。
3. 下記を設定する。
   - **料金タイプ**: `VPS割引きっぷSSLセット`
   - **利用するきっぷ**: `購入済み`
   - **プラン**: `メモリ 8GB / 2023-07-22 ~ 2024-07-22`
   - **イメージタイプ > OS**: `Ubuntu 22.04 (64bit)`
   - **root パスワード**: 【パスワード】
   - **ネームタグ**: `rsl＊＊＊`（RSL番号）
   - **オプション**
     - **自動バックアップ**: `無効`
     - **追加ディスク**: `使用しない`
     - **接続許可ポート IPv4**: `全て許可`
     - **接続許可ポート IPv6**: `全て拒否`
     - **SSH Key**: `登録済みキー`
       - **パブリックキー**: `key-conoha-rsl＊＊＊`（**SSH Keyの作成**で作成したキー）
     - **スタートアップスクリプト**: `使用しない`
4. **追加**ボタンをクリックする。

### 時間課金で新規に利用する場合
1. **サーバー追加**をクリックする。
2. **VPS**タブを開く。
3. 下記を設定する。
   - **料金タイプ**: `時間課金`
   - **プラン**: `メモリ 1GB / CPU 2Core / SSD 100GB`
   - **イメージタイプ > OS**: `Ubuntu 22.04 (64bit)`
   - **root パスワード**: 【パスワード】
   - **ネームタグ**: `rsl＊＊＊`（RSL番号）
   - **オプション**
     - **自動バックアップ**: `無効`
     - **追加ディスク**: `使用しない`
     - **接続許可ポート IPv4**: `全て許可`
     - **接続許可ポート IPv6**: `全て拒否`
     - **SSH Key**: `登録済みキー`
       - **パブリックキー**: `key-conoha-rsl＊＊＊`（**SSH Keyの作成**で作成したキー）
     - **スタートアップスクリプト**: `使用しない`
4. **追加**ボタンをクリックする。

## サーバーの起動
1. **サーバー**をクリックする。
2. **サーバーリスト**から起動したいサーバのネームタグをクリックする。
   1. **コンソール**ボタンをクリックする。
   2. 端末が開くので、下記のとおりrootでログインする。

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

**コンソール**から作成したユーザでログインしなおす。
```bash
Ubuntu 22.04.1 LTS ＊＊＊-＊＊＊-＊＊＊-＊＊＊ tty1

＊＊＊-＊＊＊-＊＊＊-＊＊＊ login: rsl
Password: 【パスワード】
rsl@＊:~$
```

## SSHの設定

### 設定ファイルの準備
```bahs
rsl@＊:~$ ls /etc/ssh/
rsl@＊:~$ less /etc/ssh/sshd_config
rsl@＊:~$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config-org
rsl@＊:~$ ls /etc/ssh/
rsl@＊:~$ mkdir ~/.ssh/
```

### 公開鍵の設置
1. **セキュリティ > SSH Key**をクリックする。
2. **SSH Key**リストから`key-conoha-rsl＊＊＊`をクリックする。
3. **パブリックキー**の内容をメモする。
4. **コンソール**から下記を実行する。

```bash
rsl@＊:~$ cat << EOF > temp.txt
```

5. **コンソール**の**テキスト送信**をクリックし、メモした**パブリックキー**の内容を貼り付けて送信する。
   - 一度にすべて送信しようとすると、送信漏れが発生する。そのため（80文字程度など）何回かに分割して送信する。
   - 1字でも誤りがあると、SSH接続できないので注意。
6. すべて送信したら、改行して`EOF`と書いてファイルに出力する。

```bash
rsl@＊:~$ tr -d '\n' < temp.txt >> ~/.ssh/authorized_keys
rsl@＊:~$ chmod 700 ~/.ssh/
rsl@＊:~$ chmod 600 ~/.ssh/authorized_keys
rsl@＊:~$ ls -l
rsl@＊:~$ ls -l ~/.ssh/
rsl@＊:~$ less ~/.ssh/authorized_keys
rsl@＊:~$ rm -f temp.txt
```

### SSHの設定（サーバー側）
```bash
rsl@＊:~$ less /etc/ssh/sshd_config
rsl@＊:~$ sudo vi /etc/ssh/sshd_config
```

`sshd_config`の下記の箇所を書き換える。
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

### ファイアウォールの設定
```bash
rsl@＊:~$ sudo ufw status
rsl@＊:~$ sudo ufw default deny
rsl@＊:~$ sudo ufw allow OpenSSH
rsl@＊:~$ sudo ufw status
rsl@＊:~$ sudo ufw reload
```

### SSHの設定（クライアント側）
```bash
$ mkdir ~/.ssh/
```

**SSH Keyの作成**でダウンロードしたプライベートキー`key-conoha-rsl＊＊＊.pem`を`~/.ssh/に置く。

```bash
$ vi ~/.ssh/config
```

`~/.ssh/config`
```bash
Host conoha_rsl＊＊＊（RSL番号）
  HostName 【サーバーのIPアドレス】
  port 22
  User rsl
  IdentityFile ~/.ssh/key-conoha-rsl＊＊＊.pem（**SSH Keyの作成**で作成したキー）
```

```bash
$ chmod 700 ~/.ssh/
$ chmod 600 ~/.ssh/config
$ chmod 400 ~/.ssh/key-conoha-rsl＊＊＊.pem
$ ls -l
$ ls -l ~/.ssh/
```

下記でサーバーにSSH接続できればOK。
```bash
$ ssh conoha_rsl＊＊＊
```

#### 参考
1. [ConoHa VPS 公開鍵認証でSSH接続 #Conoha - Qiita](https://qiita.com/kaoru0404/items/9c5ff6d45462e9d06133)

