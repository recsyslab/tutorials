# MeCabのインストール

## mecabのインストール
```bash
sudo apt install mecab libmecab-dev mecab-ipadic mecab-ipadic-utf8
```

## 設定ファイルの準備
```bash
ls /etc/
less /etc/mecabrc
sudo cp /etc/mecabrc /etc/mecabrc-org
ls /etc/
```

## mecabの実行
```bash
mecab
```

## mecab-ipadic-NEologdのインストール
```bash
cd ~/src/
git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git
cd mecab-ipadic-neologd/
sudo ./bin/install-mecab-ipadic-neologd -n -a
[install-mecab-ipadic-NEologd] : Do you want to install mecab-ipadic-NEologd? Type yes or no.
yes
# ...（1分程度）...
```

## 設定ファイルの編集
```bash
echo `mecab-config --dicdir`"/mecab-ipadic-neologd"
# ここでインストール先ディレクトリを確認する．
# 例；/usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd
ls /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd
sudo vi /etc/mecabrc
```

`mecabrc`の下記の箇所を書き換える。

`mecabrc`
```bash
...（略）...
dicdir = /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd
...（略）...
```

```bash
less /etc/mecabrc
diff /etc/mecabrc-org /etc/mecabrc
mecab
# mecabを実行し、辞書が反映されていることを確認する。
```

## 設定ファイルのコピー
PythonからMeCabを利用する際には、`usr/local/etc`を読みに行くため、`mecabrc`を`/usr/local/etc/`にコピーしておく。
```bash
sudo cp /etc/mecabrc /usr/local/etc/
```

#### 参考
- Qiita, [ubuntu 18.10 に mecab をインストール](https://qiita.com/ekzemplaro/items/c98c7f6698f130b55d53)
