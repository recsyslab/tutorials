# 本番運用環境へのDjangoプロジェクトの配置.md

## GitHubリポジトリへのpush
```bash
$ cd ~/【リポジトリ名】/
$ git add *
$ git status
$ git commit -m "（任意のメッセージ）"
$ git push
```

## サーバへの接続
```bash
$ ssh 【サーバにアクセスするユーザ名】@【サーバのIPアドレス】
【サーバのIPアドレス】$
```

## リポジトリのclone
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
