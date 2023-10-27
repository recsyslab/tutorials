# PostgreSQLとPythonのインストール

## パッケージのアップグレード
```bash
rsl@＊:~$ sudo apt update
rsl@＊:~$ sudo apt upgrade
```

## ディレクトリの準備
```bash
rsl@＊:~$ mkdir ~/src/
rsl@＊:~$ mkdir ~/opt/
rsl@＊:~$ ls
```

## PostgreSQL+PostGISのインストール

### PostgreSQLとPostGISのインストール
```bash
rsl@＊:~$ sudo apt install postgresql
rsl@＊:~$ sudo apt install postgis
# ...（3分程度）...
```

### 設定ファイルの準備
※下記の`14`の箇所はPostgreSQLのバージョンを表す。インストールされたバージョンに合わせて適宜置換すること。例えば、バージョン12.xをインストールした場合は、`14`の箇所を`12`に置換する。以下、同様。

```bash
$ sudo cp /etc/postgresql-common/createcluster.conf /etc/postgresql-common/createcluster.conf-org
$ sudo cp /etc/postgresql/14/main/pg_hba.conf /etc/postgresql/14/main/pg_hba.conf-org
$ sudo cp /etc/postgresql/14/main/postgresql.conf /etc/postgresql/14/main/postgresql.conf-org
```

### PostgreSQLサーバの起動設定
```bash
rsl@＊:~$ systemctl is-enabled postgresql
# 「enabled」と表示されれば、postgresqlの自動起動が有効になっている。
rsl@＊:~$ systemctl status postgresql
# 「Active: active (exited)」と表示されれば、postgresqlは稼働している。
```

### PostgreSQLのパスワードの設定
```bash
rsl@＊:~$ sudo -u postgres psql
```

```pgsql
postgres=# \password
Enter new password for user "postgres":
Enter it again: 
postgres=# \q
```

```bash
rsl@＊:~$ sudo vi /etc/postgresql/14/main/pg_hba.conf
```

`pg_hba.conf`の下記3箇所について`peer`を`md5`に書き換える。`vi`の代わりに`nano`で編集しても良い。

`/etc/postgresql/14/main/pg_hba.conf`
```
...（略）...
# Database administrative login by Unix domain socket
local all postgres md5
...（略）...
# "local" is for Unix domain socket connections only
local all all md5
...（略）...
# Allow replication connections from localhost, by a user with the
# replication privilege.
local replication all md5
...（略）...
```

```bash
rsl@＊:~$ sudo diff /etc/postgresql/14/main/pg_hba.conf-org /etc/postgresql/14/main/pg_hba.conf
rsl@＊:~$ sudo systemctl reload postgresql
rsl@＊:~$ systemctl status postgresql
rsl@＊:~$ sudo -u postgres psql
# 設定したパスワードを入力してログインできることを確認する。
```

### autovacuumの設定

```bash
$ sudo vi /etc/postgresql/14/main/postgresql.conf
```

`/etc/postgresql/14/main/postgresql.conf`
```
...（略）...
track_counts = on
...（略）...
autovacuum = on
...（略）...
```

```bash
rsl@＊:~$ sudo diff /etc/postgresql/14/main/postgresql.conf-org /etc/postgresql/14/main/postgresql.conf
rsl@＊:~$ sudo systemctl reload postgresql
rsl@＊:~$ systemctl status postgresql
```

## Pythonのインストール

### 事前準備
```bash
rsl@＊:~$ sudo apt install libbz2-dev # pandasのインポートに必要
rsl@＊:~$ sudo apt install python3-tk # matplotlib.show()で画像を表示する際に必要
rsl@＊:~$ sudo apt install build-essential # GDALのインストールに必要
rsl@＊:~$ sudo apt install libgdal-dev	# GDALのインストールに必要
rsl@＊:~$ sudo apt install python3-gdal	# GDALのインストールに必要
rsl@＊:~$ sudo apt install libffi-dev # scikit-learnのインポートに必要
```

## インストール
```bash
rsl@＊:~$ mkdir -p ~/opt/python/
rsl@＊:~$ cd ~/src/
rsl@＊:~$ wget https://www.python.org/ftp/python/3.11.5/Python-3.11.5.tar.xz
rsl@＊:~$ xz -dc Python-3.11.5.tar.xz| tar xfv -
rsl@＊:~$ cd Python-3.11.5/
rsl@＊:~$ ./configure --prefix=$HOME/opt/python --with-ensurepip=install
# ...（3分程度）...
rsl@＊:~$ make 2>&1 | tee make.log
# ...（3分程度）... 
rsl@＊:~$ make altinstall
# ...（2分程度）... 
rsl@＊:~$ cd ../
rsl@＊:~$ rm -f Python-3.11.5.tar.xz
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
$ source ~/venv/rsl-base/bin/activate
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

