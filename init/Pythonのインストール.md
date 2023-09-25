# Pythonのインストール

## 事前準備
```bash
$ sudo apt install libbz2-dev # pandasのインポートに必要
$ sudo apt install python3-tk # matplotlib.show()で画像を表示する際に必要
$ sudo apt install build-essential # GDALのインストールに必要
$ sudo apt install libgdal-dev	# GDALのインストールに必要
$ sudo apt install python3-gdal	# GDALのインストールに必要
$ sudo apt install libffi-dev # scikit-learnのインポートに必要
```

## インストール
```bash
$ mkdir -p ~/opt/python/
$ cd ~/src/
$ wget https://www.python.org/ftp/python/3.11.5/Python-3.11.5.tar.xz
$ xz -dc Python-3.11.5.tar.xz| tar xfv -
$ cd Python-3.11.5/
$ ./configure --prefix=$HOME/opt/python --with-ensurepip=install
# ...（3分程度）...
$ make 2>&1 | tee make.log
# ...（3分程度）... 
$ make altinstall
# ...（2分程度）... 
$ cd ../
$ rm -f Python-3.11.5.tar.xz
```

## インストール結果の確認
```bash
$ cd ~/opt/python/
$ ls
$ cd bin/
$ ls -alh
```

## パスの設定
```bash
$ echo $PATH
$ less ~/.profile
$ echo -e '\n# Pythonインストール時に追加' >> ~/.profile
$ echo 'export PATH="$HOME/opt/python/bin:$PATH"' >> ~/.profile
$ less ~/.profile
$ diff ~/.profile-org ~/.profile
$ echo $PATH
$ source ~/.profile
$ echo $PATH
```

## バージョンの確認
```bash
$ python3 --version
$ python3.10 --version
$ python3.11 --version
```

## ベースとなる仮想環境

### 仮想環境の構築とアクティベート
```bash
$ mkdir ~/venv/
$ cd ~/venv/
$ python3.11 -m venv rsl-base
$ source rsl-base/bin/activate
```

### pipのアップグレード
```bash
(rsl-base) $ pip --version
(rsl-base) $ pip install --upgrade pip
(rsl-base) $ pip --version
```

### 各種パッケージのインストール
```bash
(rsl-base) $ pip install ipython
(rsl-base) $ pip install numpy
(rsl-base) $ pip install scipy
(rsl-base) $ pip install matplotlib
(rsl-base) $ pip install pandas
(rsl-base) $ pip install scikit-learn
(rsl-base) $ pip install psycopg2-binary
(rsl-base) $ pip install tqdm
(rsl-base) $ pip install mecab-python3
(rsl-base) $ pip install requests
(rsl-base) $ pip install importnb
(rsl-base) $ pip install importlib
```

### インストール済みパッケージ一覧の確認
```bash
(rsl-base) $ pip freeze
```

### 各種パッケージのバージョンの確認
```bash
(rsl-base) $ ipython --version
(rsl-base) $ python
# プロンプトが>>>となればOK
```

```python
>>> import numpy
>>> numpy.__version__
>>> import scipy
>>> scipy.__version__
>>> import matplotlib
>>> matplotlib.__version__
>>> import pandas
>>> pandas.__version__
>>> import sklearn
>>> sklearn.__version__
>>> import psycopg2
>>> psycopg2.__version__
>>> import tqdm
>>> tqdm.__version__
>>> import importnb
>>> importnb.__version__
>>> [Ctrl+D]
```

### 仮想環境のディアクティベート
```bash
(rsl-base) $ deactivate
```

#### 参考
- Doitu.info, [既存のPython環境を壊すことなく、自分でビルドしてインストールする（altinstall）](https://doitu.info/blog/5c45e5ec8dbc7a001af33ce8)
- 組み込みの人。, [makeコマンドのちょっとしたtips](https://embedded.hatenadiary.org/entry/20090416/p1)
