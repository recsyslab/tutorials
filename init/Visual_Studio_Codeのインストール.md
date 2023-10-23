# Visual Studio Codeのインストール

## インストール
```bash
$ cd ~/src/
$ sudo apt install libsecret-1-0
$ wget 'https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64' -O code_latest_amd64.deb
$ sudo dpkg -i code_latest_amd64.deb
$ rm -f code_latest_amd64.deb
```

## 実行
```bash
$ code
```

## バージョンの確認
```bash
$ code -v
```
