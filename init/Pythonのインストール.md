# Pythonのインストール

## 事前準備
```bash
sudo apt install libbz2-dev # pandasのインポートに必要
sudo apt install python3-tk # matplotlib.show()で画像を表示する際に必要
sudo apt install libgdal-dev	# GDALのインストールに必要
sudo apt install python3-gdal	# GDALのインストールに必要
sudo apt install libffi-dev # scikit-learnのインポートに必要
# ...（5分程度）...
```

## インストール
```bash
mkdir -p ~/opt/python/
cd ~/src/
wget https://www.python.org/ftp/python/3.9.5/Python-3.9.5.tar.xz
xz -dc Python-3.9.5.tar.xz| tar xfv -
cd Python-3.9.5/
./configure --prefix=/home/rsl/opt/python --with-ensurepip=install
# ...（3分程度）...
make 2>&1 | tee make.log
# ...（3分程度）... 
make altinstall
# ...（2分程度）... 
cd ../
rm -f Python-3.9.5.tar.xz
```

## インストール結果の確認
```bash
cd ~/opt/python/
ls
cd bin/
ls -alh
```

## パスの設定
```bash
echo $PATH
less ~/.profile
echo -e '\n# Pythonインストール時に追加' >> ~/.profile
echo 'export PATH="$HOME/opt/python/bin:$PATH"' >> ~/.profile
less ~/.profile
diff ~/.profile-org ~/.profile
echo $PATH
source ~/.profile
echo $PATH
```

## バージョンの確認
```bash
python3 --version
python3.8 --version
python3.9 --version
```

#### 参考
- Doitu.info, [既存のPython環境を壊すことなく、自分でビルドしてインストールする（altinstall）](https://doitu.info/blog/5c45e5ec8dbc7a001af33ce8)
- 組み込みの人。, [makeコマンドのちょっとしたtips](https://embedded.hatenadiary.org/entry/20090416/p1)
