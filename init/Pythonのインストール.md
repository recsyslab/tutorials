# Pythonのインストール

## 事前準備
```bash
$ sudo apt install libbz2-dev       # pandasのインポートに必要
$ sudo apt install python3-tk       # matplotlib.show()で画像を表示する際に必要
$ sudo apt install build-essential  # GDALのインストールに必要
$ sudo apt install libgdal-dev      # GDALのインストールに必要
# ...（1分程度）...
$ sudo apt install python3-gdal	    # GDALのインストールに必要
$ sudo apt install libffi-dev       # scikit-learnのインポートに必要
```

## インストール
```bash
$ mkdir -p ~/opt/python/
$ cd ~/src/
$ wget https://www.python.org/ftp/python/3.12.6/Python-3.12.6.tar.xz
$ xz -dc Python-3.12.6.tar.xz| tar xfv -
$ cd Python-3.12.6/
$ ./configure --prefix=$HOME/opt/python --with-ensurepip=install
# ...（3分程度）...
$ make 2>&1 | tee make.log
# ...（3分程度）... 
$ make altinstall
# ...（2分程度）... 
$ rm -f ~/src/Python-3.12.6.tar.xz
```

## インストール結果の確認
```bash
$ ls ~/opt/python/
bin  include  lib  share
$ ls ~/opt/python/bin/ -alh
合計 31M
drwxr-xr-x 2 rsl rsl 4.0K  9月 10 10:19 .
drwxrwxr-x 6 rsl rsl 4.0K  9月 10 10:19 ..
-rwxr-xr-x 1 rsl rsl  112  9月 10 10:19 2to3-3.12
-rwxr-xr-x 1 rsl rsl  110  9月 10 10:19 idle3.12
-rwxrwxr-x 1 rsl rsl  240  9月 10 10:19 pip3.12
-rwxr-xr-x 1 rsl rsl   95  9月 10 10:19 pydoc3.12
-rwxr-xr-x 1 rsl rsl  31M  9月 10 10:18 python3.12
-rwxr-xr-x 1 rsl rsl 3.0K  9月 10 10:19 python3.12-config
```

## パスの設定
```bash
$ echo $PATH
/home/rsl/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ less ~/.profile
$ echo -e '\n# Pythonインストール時に追加' >> ~/.profile
$ echo 'export PATH="$HOME/opt/python/bin:$PATH"' >> ~/.profile
$ less ~/.profile
$ diff ~/.profile-org ~/.profile
27a28,33
> 
> 
> #### #### Add below. #### ####
> 
> # Pythonインストール時に追加
> export PATH="$HOME/opt/python/bin:$PATH"
$ echo $PATH
/home/rsl/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ source ~/.profile
$ echo $PATH
/home/rsl/opt/python/bin:/home/rsl/bin:/home/rsl/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

## バージョンの確認
```bash
$ python3 --version
Python 3.12.3
$ python3.12 --version
Python 3.12.6
```

## ベースとなる仮想環境

### 仮想環境の構築とアクティベート
```bash
$ cd
$ mkdir ~/venv/
$ python3.12 -m venv ~/venv/rsl_base
$ source ~/venv/rsl_base/bin/activate
(rsl_base) rsl@recsyslab-mint:~$
```

仮想環境にアクティベートすると、プロンプトの先頭に`(rsl_base)`のように仮想環境名が表示される。以降、`(【仮想環境】) $`と記載している箇所は、`【仮想環境】`にアクティベートした状態で入力するコマンドを表す。

### pipのアップグレード
```bash
(rsl_base) $ pip --version
(rsl_base) $ pip install --upgrade pip
(rsl_base) $ pip --version
pip 24.2 from /home/rsl/venv/rsl_base/lib/python3.12/site-packages/pip (python 3.12)
```

### 各種パッケージのインストール
```bash
(rsl_base) $ pip install ipython
(rsl_base) $ pip install numpy
(rsl_base) $ pip install scipy
(rsl_base) $ pip install matplotlib
(rsl_base) $ pip install pandas
(rsl_base) $ pip install scikit-learn
(rsl_base) $ pip install psycopg2-binary
(rsl_base) $ pip install tqdm
(rsl_base) $ pip install timedelta
(rsl_base) $ pip install requests
(rsl_base) $ pip install importnb
(rsl_base) $ pip install importlib
```

### インストール済みパッケージ一覧の確認
```bash
(rsl_base) $ pip freeze
```

### 各種パッケージのバージョンの確認
```bash
(rsl_base) $ ipython --version
(rsl_base) $ python
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
>>> import requests
>>> requests.__version__
>>> import importnb
>>> importnb.__version__
>>> [Ctrl+D]
```

### 仮想環境のディアクティベート
```bash
(rsl_base) $ deactivate
$
```

仮想環境をディアクティベートすると、プロンプトが元に戻る。

#### 参考
1. Doitu.info, [既存のPython環境を壊すことなく、自分でビルドしてインストールする（altinstall）](https://doitu.info/blog/5c45e5ec8dbc7a001af33ce8)
1. 組み込みの人。, [makeコマンドのちょっとしたtips](https://embedded.hatenadiary.org/entry/20090416/p1)
