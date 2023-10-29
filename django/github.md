# GitHub

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
         - **Repository name**: `rsl＊＊＊`（`＊＊＊`はRSL番号）
         - **Public/Private**: `Private`
         - **Add a README file**: `チェック`
         - **Add .gitignore**: （任意）
         - **Choose a license**: （任意）
      2. **Create repository**ボタンをクリックする。

## GitとSSHのインストール
```bash
$ sudo apt install git
```

## 招待メールからrecsyslabプロジェクトに参加
1. GitHubからの招待メールの案内にしたがって、メール本文内の**Join@recsyslab**ボタンをクリックする。
   1. 下記を設定する。
      - **Pick a username**: `rsl＊＊＊`（`＊＊＊`はRSL番号）
      - **Your email address**: `y＊＊＊＊＊＊@mail.ryukoku.ac.jp`（招待された大学のメールアドレス）
      - **Password**: 【自分のわかるパスワード】
   2. **Create account and join**ボタンをクリックする。
   3. アカウントを検証し、**Join a free plan**ボタンをクリックする。
   4. プロジェクト画面が表示される。

## SSH Keyの設定
```bash
$ ssh-keygen -t rsa -C "y＊＊＊＊＊＊@mail.ryukoku.ac.jp"（招待された大学のメールアドレス）
ssh-keygen -t rsa -C "okukenta@rins.ryukoku.ac.jp"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rsl/.ssh/id_rsa): /home/rsl/.ssh/github-rsl＊＊＊.key（＊＊＊はRSL番号）
Enter passphrase (empty for no passphrase): 【パスフレーズ】
Enter same passphrase again: 【パスフレーズ】
Your identification has been saved in /home/rsl/.ssh/github-rsl＊＊＊.key
Your public key has been saved in /home/rsl/.ssh/github-rsl＊＊＊.key.pub
The key fingerprint is:
...（略）...
$ ls ~/.ssh/
$ cat ~/.ssh/github-rsl＊＊＊.key.pub
```

## 公開鍵の登録
1. GitHubの右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: 【任意の鍵の名前】
         - **Key**: `github-rsl＊＊＊.key.pub`の内容を貼り付ける。※`github-rsl＊＊＊.key`ではないので注意
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

## SSHの設定
```bash
$ vi ~/.ssh/config
```

`~/.ssh/config`
```bash
...（略）...
Host github
  HostName github.com
  User git
  IdentityFile ~/.ssh/github-rsl＊＊＊.key  
```

```bash
$ less ~/.ssh/config 
$ chmod 400 ~/.ssh/github-rsl＊＊＊.key
$ ls -l ~/.ssh/
```

## SSH接続の確認
下記コマンドで次のようなメッセージが表示されれれば接続成功。
```bash
$ ssh -T github
Enter passphrase for key '/home/rsl/.ssh/github-rsl＊＊＊.key': 【パスフレーズ】
Hi y＊＊＊＊＊＊! You've successfully authenticated, but GitHub does not provide shell access.
```



## リポジトリにアクセス
下記URLでリポジトリにアクセスできる。
- `https://github.com/recsyslab/【リポジトリ名】`（【リポジトリ名】はRSL番号、例；rsl000）

## リポジトリのclone
1. リポジトリのトップページの**Code - SSH**タブからcloneする際に指定するパスを確認する。
   - `git clone git@github.com:recsyslab/【リポジトリ名】.git`
```bash
$ cd
$ mkdir -p 【リポジトリ名】/
$ git clone git@github.com:recsyslab/【リポジトリ名】.git
$ cd 【リポジトリ名】/
$ ls
```

## リポジトリのpull
```bahs
$ cd 【リポジトリ名】/
$ git pull
```

## Djangoプロジェクトのコピー
```bash
$ cd
$ cp -r 【Djangoプロジェクト名】/ 【リポジトリ名】/
$ ls 【リポジトリ名】/
```

## push
```bash
$ cd ~/【リポジトリ名】/
$ git add 【Djangoプロジェクト名】/
$ git status
$ git commit -m "add 【Djangoプロジェクト名】/"
$ git push
```

リポジトリにアクセスし、Djangoプロジェクトが配置されていればOK。



