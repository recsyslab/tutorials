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
         - **Repository name**: 【リポジトリ名】（RSL番号、例；rsl000）
         - **Public/Private**: `Private`
         - **Add a README file**: `チェック`
         - **Add .gitignore**: （任意）
         - **Choose a license**: （任意）
      2. **Create repository**ボタンをクリックする。

## GitとSSHのインストール
```bash
$ sudo apt install git
$ sudo apt install ssh
```

## 招待メールからrecsyslabプロジェクトに参加
1. GitHubからの招待メールの案内にしたがって、メール本文内の**Join@recsyslab**ボタンをクリックする。
   1. 下記を入力して**Create account and join**ボタンをクリックする。
   2. Pick a username: （RSL番号、例；rsl000）
   3. Your email address: （招待された大学のメールアドレス）
   4. Password: （自分のわかるパスワード）
   5. アカウントを検証し、**Join a free plan**ボタンをクリックする。
   6. プロジェクト画面が表示される。

## SSH Keyの設定
```bash
$ ssh-keygen -t rsa -C "【メールアドレス】"
$ ls ~/.ssh/
$ cat ~/.ssh/id_rsa.pub
```

## 公開鍵の登録
1. GitHubの右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: 【任意の鍵の名前】
         - **Key**: `id_rsa.pub`の内容を貼り付ける。
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

## 動作確認
```bash
$ ssh -T git@github.com
```

次のように表示されれば成功
```bash
Hi 【ユーザ名】! You've successfully authenticated, but GitHub does not provide shell access.
```




## リポジトリにアクセス
下記URLでリポジトリにアクセスできる。
- `https://github.com/recsyslab/【リポジトリ名】`（【リポジトリ名】はRSL番号、例；rsl000）

## リポジトリのclone
1. リポジトリのトップページの**Code - SSH**タブからcloneする際に指定するパスを確認する。
   - `git clone git@github.com:recsyslab/【リポジトリ名】.git`
3. 端末で下記を実行する。
```bash
$ cd
$ mkdir -p 【リポジトリ名】/
$ git clone git@github.com:recsyslab/【リポジトリ名】.git
$ cd 【リポジトリ名】/
$ ls
```

## リポジトリのpull
```bahs
$ pull
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

