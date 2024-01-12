# Webシステム開発環境の構築

## プロジェクトのホームディレクトリの作成
```bash
$ cd /usr/local/src/
$ sudo mkdir dev/
$ sudo chmod 777 dev/
$ ls -l
$ mkdir -p dev/app/
$ mkdir -p dev/app/frontend/
$ mkdir -p dev/app/backend/
```

## フロントエンドのコンテナの構築
```bash
$ systemctl status docker
# Dockerが起動していることを確認する。
$ cd /usr/local/src/dev/app/frontend/
$ code .
```

1. VSCodeの左下の**Manage > Command Palette**を開く。
   1. 「containers」で検索する。
   2. 「Dev Containers: Open Folder in Container」を選択する。
   3. 現在のディレクトリ`/usr/local/src/dev/app/frontend/`が表示されていることを確認し、**開く**ボタンをクリックする。
   4. コンテナー構成テンプレととして「Node.js」を選択する。
   5. バージョン「18」を選択する。
   6. **OK**ボタンをクリックする。

## Next.jsのアプリケーションのインストール
1. VSCodeの上部メニューから**ターミナル > 新しいターミナル**を開く。
   1. ターミナルから下記コマンドを実行する。

```bash
$ yarn create next-app frontend --ts --esling
# 何度か質問されるがデフォルトのまま進める。
```
