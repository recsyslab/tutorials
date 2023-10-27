# PostgreSQLとPythonのインストール

## 準備

### パッケージのアップグレード
```bash
rsl@＊:~$ sudo apt update
rsl@＊:~$ sudo apt upgrade
```

### ディレクトリの準備
```bash
rsl@＊:~$ mkdir ~/src/
rsl@＊:~$ mkdir ~/opt/
rsl@＊:~$ ls
```

### 設定ファイルの準備
```bash
$ cp ~/.profile ~/.profile-org
$ echo -e '\n\n#### #### Add below. #### ####' >> ~/.profile
$ diff ~/.profile-org ~/.profile
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

### インストール
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

### インストール結果の確認
```bash
rsl@＊:~$ cd ~/opt/python/
rsl@＊:~$ ls
rsl@＊:~$ cd bin/
rsl@＊:~$ ls -alh
```

### パスの設定
```bash
rsl@＊:~$ echo $PATH
rsl@＊:~$ echo -e '\n# Pythonインストール時に追加' >> ~/.profile
rsl@＊:~$ echo 'export PATH="$HOME/opt/python/bin:$PATH"' >> ~/.profile
rsl@＊:~$ diff ~/.profile-org ~/.profile
rsl@＊:~$ source ~/.profile
rsl@＊:~$ echo $PATH
```

### バージョンの確認
```bash
rsl@＊:~$ python3 --version
rsl@＊:~$ python3.10 --version
rsl@＊:~$ python3.11 --version
```
