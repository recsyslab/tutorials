# GitHubの導入

## GitHubアカウントの作成
1. 下記からGitHubアカウントを作成する。
   - GitHub: https://github.com/join/

## SSH Keyの設定
```bash
ssh-keygen -t rsa -C "【メールアドレス】"
ls ~/.ssh/
cat ~/.ssh/id_rsa.pub
```

## 公開鍵の登録
1. GitHubの右上のアカウント設定ボタンから**Settings**を開く。
   1. **SSH and GPG Keys**を開く。
      1. **New SSH Key**ボタンをクリックし、下記を設定する。
         - **Title**: 【任意の鍵の名前】
         - **Key**: `id_rsa.pub`の内容
      2. **Add SSH Key**ボタンをクリックする。
         - 成功すると登録したメールアドレスに公開鍵登録完了メールが届く。

## 動作確認
```bash
ssh -T git@github.com
```

次のように表示されれば成功
```bash
Hi 【ユーザ名】! You've successfully authenticated, but GitHub does not provide shell access.
```
