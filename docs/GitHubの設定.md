# GitHubの設定

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
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: `y＊＊＊＊＊＊@rsl-local`
         - **Key**: `id_rsa.pub`の内容を貼り付ける。※`id_rsa`ではないので注意
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

### SSH接続の確認

下記コマンドで次のようなメッセージが表示されれれば接続成功。

```bash
$ ssh -T git@github.com
Enter passphrase for key '/home/rsl/.ssh/id_rsa': 【パスフレーズ】
Hi y＊＊＊＊＊＊! You've successfully authenticated, but GitHub does not provide shell access.
```

## Gitのインストール

```bash
$ sudo apt install git
```

## リポジトリのclone

1. 下記URLでリポジトリにアクセスする。
   - `https://github.com/recsyslab/rsl＊＊＊`（`rsl＊＊＊`はRSL番号）
2. リポジトリのトップページの**Code - SSH**タブからcloneする際に指定するパスをコピーする。
   - `git clone git@github.com:recsyslab/rsl＊＊＊.git`
3. 下記コマンドでリポジトリをcloneする。

```bash
$ cd
$ git clone git@github.com:recsyslab/rsl＊＊＊.git（上記でコピーしたパス）
$ cd ~/rsl＊＊＊/
$ ls
```

## リポジトリへのDjangoプロジェクトのpush

### リポジトリのpull

```bahs
$ cd ~/rsl＊＊＊/
$ git pull
```

### Djangoプロジェクトのコピー

※ここでは、ホームディレクトリに`recsys_django`プロジェクトが作成されているとする。

```bash
$ cp -r ~/recsys_django/ ~/rsl＊＊＊/
$ ls ~/rsl＊＊＊/
```

### push

```bash
$ cd ~/rsl＊＊＊/
$ git add recsys_django/
$ git status
$ git commit -m "add recsys_django/"
$ git push
```

GitHubの`rsl＊＊＊`リポジトリにアクセスし、`recsys_django`プロジェクトが配置されていればOK。
