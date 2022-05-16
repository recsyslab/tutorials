# PyCharm

## ダウンロード
1. 下記からPyCharmをダウンロードする。
   - **[PyCharm](http://www.jetbrains.com/pycharm/)**
   - **PyCharm**: `pycharm-community-2020.2.tar.gz`

## インストール
```bash
cd ~/Download/
tar xvzf pycharm-community-2022.1.1.tar.gz
mv pycharm-community-2022.1.1/ pycharm-community/
mv pycharm-community ~/opt/
sudo ln -s ~/opt/pycharm-community/bin/pycharm.sh /usr/local/bin/pycharm
sudo ln -s ~/opt/pycharm-community/bin/inspect.sh /usr/local/bin/inspect
```

## 起動
```bash
pycharm
```

## 後始末
```bash
rm -f pycharm-community-2022.1.1.tar.gz
```
