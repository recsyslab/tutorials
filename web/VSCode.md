# VSCode

## VSCodeとコンテナの連携

### Dev Containersのインストール
1. VSCodeの左メニューから**Extensions**を開く。
   1. 「containers」で検索する。
   2. 「Dev Containers」を選択する。
   3. **Install**ボタンをクリックする。

### yarnのインストール
1. VSCodeの左メニューから**Extensions**を開く。
   1. 「yarn」で検索する。
   2. 「yarn」を選択する。
   3. **Install**ボタンをクリックする。

## プロジェクトのホームディレクトリの作成
```bash
$ cd
$ mkdir ~/dev/
$ sudo chmod 777 ~/dev/
$ ls -l
$ mkdir p ~/dev/app/
$ mkdir -p ~/dev/app/frontend/
$ mkdir -p ~/dev/app/backend/
```

## フロントエンドのコンテナの構築
```bash
$ cd ~/dev/app/frontend/
$ code .
```

1. VSCodeの左下の**Manage > Command Palette**を開く。
   1. 「containers」で検索する。
   2. 「Dev Containers: Open Folder in Container」を選択する。
