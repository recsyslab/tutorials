# GitHubリポジトリへのDjangoプロジェクトのpush

## 準備（教員の作業）

### GitHubに招待
1. `recsyslab`プロジェクトトップページのメニューから**People**を開く。
   1. **Invite member**ボタンをクリックする。
   2. **Invite a member to recsyslab**に招待したいメールアドレスを入力する。
   3. **Invite**ボタンをクリックする。
   4. **Role in the organization**で`Member`を選択し、**Send invitation**ボタンをクリックする。

### 学生用リポジトリの作成
1. organizationsのトップページから、**Repositories**を開く。
   1. **New repository**ボタンをクリックする。
      1. 下記を設定する。
         - **Owner**: `recsyslab`
         - **Repository name**: `rsl＊＊＊`（`rsl＊＊＊`はRSL番号）
         - **Public/Private**: `Private`
         - **Add a README file**: `チェック`
         - **Add .gitignore**: （任意）
         - **Choose a license**: （任意）
      2. **Create repository**ボタンをクリックする。

## 招待メールからのrecsyslabプロジェクトへの参加
1. GitHubからの招待メールの案内にしたがって、メール本文内の**Join@recsyslab**ボタンをクリックする。
   1. 下記を設定する。
      - **Pick a username**: `rsl＊＊＊`（`rsl＊＊＊`はRSL番号）
      - **Your email address**: `y＊＊＊＊＊＊@mail.ryukoku.ac.jp`（招待された大学のメールアドレス）
      - **Password**: 【自分のわかるパスワード】
   2. **Create account and join**ボタンをクリックする。
   3. アカウントを検証し、**Join a free plan**ボタンをクリックする。
   4. プロジェクト画面が表示される。

## SSH接続の設定

### SSH Keyの設定
```bash
$ ssh-keygen -t rsa -C "y＊＊＊＊＊＊@mail.ryukoku.ac.jp"（招待された大学のメールアドレス）
ssh-keygen -t rsa -C "okukenta@rins.ryukoku.ac.jp"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_rsa): （デフォルトのままEnterキーを押す）
Enter passphrase (empty for no passphrase): 【パスフレーズ】
Enter same passphrase again: 【パスフレーズ】
Your identification has been saved in /home/rsl/.ssh/id_rsa
Your public key has been saved in /home/rsl/.ssh/id_rsa.pub
The key fingerprint is:
...（略）...
$ ls ~/.ssh/
$ cat ~/.ssh/id_rsa_pub
```

### 公開鍵の登録
1. [GitHub Dashboard](https://github.com/dashboard)の右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: 【任意の鍵の名前】
         - **Key**: `id_rsa.pub`の内容を貼り付ける。※`id_rsa`ではないので注意
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

### SSH接続の確認
下記コマンドで次のようなメッセージが表示されれれば接続成功。
```bash
$ ssh -T git@github.com
Enter passphrase for key '/home/rsl/.ssh/id_rsa': 
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
