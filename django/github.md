# GitHub

## GitHubに招待（教員の作業）
1. `recsyslab`プロジェクトトップページのメニューから**People**を開く。
   1. **Invite member**ボタンをクリックする。
   2. **Invite a member to recsyslab**に招待したいメールアドレスを入力する。
   3. **Invite**ボタンをクリックする。
   4. **Role in the organization**で`Member`を選択し、**Send invitation**ボタンをクリックする。

## 学生用リポジトリの作成（教員の作業）
1. organizationsのトップページから、**Repositories**を開く。
   1. **New repository**ボタンをクリックする。
      1. 下記を設定する。
         - **Owner**: `recsyslab`
         - **Repository name**: （RSL番号，例；rsl000）
         - **Public/Private**: `Private`
         - **Add a README file**: `チェック`
         - **Add .gitignore**: （任意）
         - **Choose a license**: （任意）
      2. **Create repository**ボタンをクリックする。

## 招待メールからrecsyslabプロジェクトに参加
1. GitHubからの招待メールの案内にしたがって、メール本文内の**Join@recsyslab**ボタンをクリックする。
   1. 下記を入力して**Create account and join**ボタンをクリックする。
   2. Pick a username: （RSL番号，例；rsl000）
   3. Your email address: （招待された大学のメールアドレス）
   4. Password: （自分のわかるパスワード）
   5. アカウントを検証し、**Join a free plan**ボタンをクリックする。
   6. プロジェクト画面が表示される。

## リポジトリにアクセス
下記URLでリポジトリにアクセスできる。
- `https://github.com/recsyslab/（RSL番号，例；rsl000）`

## リポジトリのclone
1. リポジトリのトップページの**Code - SSH**タブからcloneする際に指定するパスを確認する。

```bash
mkdir -p 【リポジトリ名】/
git clone git@github.com:recsyslab/【リポジトリ名】.git
cd 【リポジトリ名】/
ls
```
