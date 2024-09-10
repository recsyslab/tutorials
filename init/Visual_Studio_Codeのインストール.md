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
1.93.0
4849ca9bdf9666755eb463db297b69e5385090e3
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
