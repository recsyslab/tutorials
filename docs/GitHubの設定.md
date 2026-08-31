# GitHubの設定

## パッケージのインストール

### sshのインストール

```bash
$ sudo apt install ssh
$ ssh -V
OpenSSH_9.6p1 Ubuntu-3ubuntu13.18, OpenSSL 3.0.13 30 Jan 2024
```

### gitのインストール

```bash
$ sudo apt install git
$ git --version
git version 2.43.0
```

## SSH接続の設定

### SSH Keyの設定

```bash
$ ssh-keygen -t ed25519 -C "y＊＊＊＊＊＊@mail.ryukoku.ac.jp" # 招待された大学のメールアドレス
$ ssh-keygen -t ed25519 -C "y＊＊＊＊＊＊@mail.ryukoku.ac.jp"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_ed25519): # デフォルトのままEnterキーを押す。
Enter passphrase (empty for no passphrase): # 任意のパスワードを入力する。
Enter same passphrase again:
Your identification has been saved in /home/rsl/.ssh/id_ed25519
Your public key has been saved in /home/rsl/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256: ...（略）... y＊＊＊＊＊＊@mail.ryukoku.ac.jp
The key's randomart image is:
+--[ED25519 256]--+
...（略）...
+----[SHA256]-----+

$ ls ~/.ssh/
id_ed25519  id_ed25519.pub  known_hosts

$ cat ~/.ssh/id_ed25519.pub
ssh-ed25519 ...（略）... y＊＊＊＊＊＊@mail.ryukoku.ac.jp
```

### 公開鍵の登録

1. [GitHub Dashboard](https://github.com/dashboard)の右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックする。
      2. 下記を設定する。
         - **Title**: `rsl@rsl＊＊＊-mint`
         - **Key**: `id_ed25519.pub`の内容を貼り付ける。 # `id_ed25519`ではないので注意
      3. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

### SSH接続の確認

下記コマンドで次のようなメッセージが表示されれれば接続成功。

```bash
$ ssh -T git@github.com
Hi y＊＊＊＊＊＊! You've successfully authenticated, but GitHub does not provide shell access.
```

## リポジトリのclone

1. 下記URLでリポジトリにアクセスする。
   - `https://github.com/recsyslab/rsl＊＊＊` # `rsl＊＊＊`はRSL番号
   1. リポジトリのトップページから**Code**を開く。
      1. **SSH**タブからSSHのURLをコピーする。
         - **URL**: `git@github.com:recsyslab/rsl＊＊＊.git`
2. 下記コマンドでリポジトリをcloneする。

```bash
$ mkdir ~/dev/
$ cd ~/dev/
$ git clone git@github.com:recsyslab/rsl＊＊＊.git # 上記でコピーしたURL
Cloning into 'rsl＊＊＊'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (3/3), done.

$ cd rsl＊＊＊/
$ ls
README.md
```

## Author identityの設定

```bash
$ git config --global user.email "y＊＊＊＊＊＊@mail.ryukoku.ac.jp"
$ git config --global user.name "rsl＊＊＊"
```
