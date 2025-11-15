# Visual Studio Codeのインストール

## インストール
```bash
$ cd ~/src/
$ sudo apt install libsecret-1-0
$ wget 'https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64' -O code_latest_amd64.deb
$ sudo dpkg -i code_latest_amd64.deb
# 「Add Microsoft apt repository for Visual Studio Code?」: <はい>
$ rm -f code_latest_amd64.deb
```

## バージョンの確認
```bash
$ code -v
1.106.0
ac4cbdf48759c7d8c3eb91ffe6bb04316e263c57
x64
```

## 起動
```bash
$ code
```

## VSCodeの日本語化
1. VSCodeの左メニューから**Extensions**を開く。
   1. 「japanese」で検索する。
   2. **Japanese Language Pack for Visual Studio Code**を選択する。
   3. **Install**ボタンをクリックする。
2. VSCodeの左下の**Manage > Command Palette**を開く。
   1. 「display」で検索する。
   2. **Configure Display Language**を選択する。
   3. **日本語**を選択する。
   4. **Restart**ボタンをクリックする。

## プラグインのインストール
1. VSCodeの左メニューから**拡張機能**を開き、下記の各プラグインをインストールする。
   - **GitHub Copilot - GitHub**

## GitHub Copilot
1. VSCode右下の**GitHub Copilot**アイコンをクリックする。
2. **Sign in to use AI Features**ボタンをクリックする。
5. **Continue with GitHub**ボタンをクリックする。
6. **Continue**ボタンをクリックする。
7. **Authorize Visual-Studio-Code**ボタンをクリックする。

#### 参考
1. [【VsCode】Unity＆C#を効率よく開発するための拡張機能のすゝめ](https://zenn.dev/tmb/articles/1444e0a85543e5)
2. [VSCode ではじめる GitHub Copilot 活用術 #githubcopilot - Qiita](https://qiita.com/RyoWakabayashi/items/1207128e88669c76bf5f)

