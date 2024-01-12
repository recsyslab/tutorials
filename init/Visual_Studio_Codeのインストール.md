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

## VSCodeの日本語化
1. VSCodeの左メニューから**Extensions**を開く。
   1. 「japanese」で検索する。
   2. 「Japanese Language Pack for VS Code」を選択する。
   3. **Install**ボタンをクリックする。
2. VSCodeの左下の**Manage > Command Palette**を開く。
   1. 「display」で検索する。
   2. 「Configure Display Language」を選択する。
   3. 「日本語」を選択する。
   4. **Restart**ボタンをクリックする。

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

#### 参考
- 株式会社オープントーン，佐藤大輔，伊東直喜，上野啓二，『実装で学ぶフルスタックWeb開発 エンジニアの視野と知識を広げる「一気通貫」型ハンズオン』，翔泳社，2023．
