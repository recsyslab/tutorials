# Google Chromeのインストール

## ダウンロード
1. 下記からdebパッケージをダウンロードする。
   - **[Google Chrome](https://www.google.com/intl/ja/chrome/)**
   - **Chrome for Linux**: `google-chrome-stable_current_amd64.deb`

## インストール
```bash
$ cd ~/Downloads/
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
  # 依存関係エラーが発生したら、関係するライブラリをインストールする。
```

## バージョンの確認
```bash
google-chrome --version
```

## 起動
```bash
google-chrome
```

## 後始末
```bash
rm -f google-chrome-stable_current_amd64.deb
```
