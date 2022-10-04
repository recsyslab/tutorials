# Django環境の構築

## パッケージの更新
```bash
sudo apt update
sudo apt upgrade
```

## ディレクトリの準備
- `~/bin/`: 独自のシェルスクリプトを置いておく。
- `~/src/`: ソースからインストールする際に必要なファイルを置いておく。
- `~/opt/`: 追加したアプリケーションを置いておく。
```bash
mkdir ~/bin/
mkdir ~/src/
mkdir ~/opt/
```

## Google Chromeのインストール

### ダウンロード
1. 下記からdebパッケージをダウンロードする。
   - **[Google Chrome](https://www.google.com/intl/ja/chrome/)**
   - **Chrome for Linux**: `google-chrome-stable_current_amd64.deb`

### インストール
```bash
cd ~/Downloads/
sudo dpkg -i google-chrome-stable_current_amd64.deb
# 依存関係エラーが発生したら、関係するライブラリをインストールする。
```

### バージョンの確認
```bash
google-chrome --version
```

### 起動
```bash
google-chrome
```

### 後始末
```bash
rm -f google-chrome-stable_current_amd64.deb
```

## PostgreSQLのインストール

### PostgreSQL 12のインストール
```bash
sudo apt install postgresql
```

### 設定ファイルの準備
`createcluster.conf`
```bash
ls /etc/postgresql-common/
less /etc/postgresql-common/createcluster.conf
sudo cp /etc/postgresql-common/createcluster.conf /etc/postgresql-common/createcluster.conf-org
ls /etc/postgresql-common/
```

`pg_hba.conf`
```bash
ls /etc/postgresql/12/main/
sudo less /etc/postgresql/12/main/pg_hba.conf
sudo cp /etc/postgresql/12/main/pg_hba.conf /etc/postgresql/12/main/pg_hba.conf-org
ls /etc/postgresql/12/main/
```

## PostgreSQLの動作確認とバージョンの確認
```bash
sudo -u postgres psql
```

```pgsql
SELECT version();
# インストールしたバージョンが表示されればOK。
\q
# '\'はキーボードの右下のバックスラッシュ「ろ」を押す（右上の'￥'ではない）
```

## PostgreSQLサーバの起動
```bash
sudo service postgresql start
service postgresql status
```

## PostgreSQLのパスワードの設定
```bash
sudo -u postgres psql
```

```pgsql
\password
\q
```

```bash
sudo less /etc/postgresql/12/main/pg_hba.conf
sudo vi /etc/postgresql/12/main/pg_hba.conf
```
`pg_hba.conf`の下記3箇所について`peer`を`md5`に書き換える。

`pg_hba.conf`
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
sudo less /etc/postgresql/12/main/pg_hba.conf
sudo diff /etc/postgresql/12/main/pg_hba.conf-org /etc/postgresql/12/main/pg_hba.conf
sudo service postgresql restart
sudo -u postgres psql
# 設定したパスワードを入力してログインできることを確認する。
```

#### 参考
- Qiita, [peer認証の関係でpsqlログインできない時の対処法](https://qiita.com/tomlla/items/9fa2feab1b9bd8749584)
- 黒い猫の開発日記, [PostGISを利用してみる。](https://cats-mew.hatenadiary.org/entry/20090811/1249976482)
  - ただし、`GeomFromText`は`ST_GeomFromText`に、`AsText`は`ST_AsText`に読み替える必要がある。

## Pythonのインストール

### 事前準備
```bash
sudo apt install libbz2-dev # pandasのインポートに必要
sudo apt install python3-tk # matplotlib.show()で画像を表示する際に必要
sudo apt install build-essential # GDALのインストールに必要
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
./configure --prefix=$HOME/opt/python --with-ensurepip=install
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
ls -a ~/
less ~/.profile
cp ~/.profile ~/.profile-org
ls -a ~/
echo $PATH
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

## 仮想環境の構築

### 仮想環境の構築とアクティベート
```bash
mkdir ~/venv/
cd ~/venv/
python3.9 -m venv rsl-django
source rsl-django/bin/activate
```

### pipのアップグレード
```bash
(rsl-django) $ pip --version
(rsl-django) $ pip install --upgrade pip
(rsl-django) $ pip --version
```

### 各種パッケージのインストール
```bash
(rsl-django) $ pip install ipython
(rsl-django) $ pip install numpy
(rsl-django) $ pip install scipy
(rsl-django) $ pip install matplotlib
(rsl-django) $ pip install pandas
(rsl-django) $ pip install scikit-learn
(rsl-django) $ pip install psycopg2-binary
(rsl-django) $ pip install tqdm
(rsl-django) $ pip install mecab-python3
(rsl-django) $ pip install requests
```

### django関連パッケージのインスト―ル
```bash
(rsl-django) $ pip install django
(rsl-django) $ pip install django-leaflet
(rsl-django) $ export CPLUS_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ export C_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ pip install gdal==3.0.4 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
(rsl-django) $ pip install djangorestframework-gis # RESTful APIに必要
(rsl-django) $ pip install django-filter # RESTful APIに必要
(rsl-django) $ pip install markdown # RESTful APIに必要
(rsl-django) $ pip install django-bootstrap5
(rsl-django) $ pip install django-allauth
(rsl-django) $ pip install django-cleanup
```

### インストール済みパッケージ一覧の確認
```bash
(rsl-django) $ pip freeze
```

## Djangoプロジェクトとアプリケーションの作成

### Djangoプロジェクトの作成
```bash
(rsl-django) $ cd
(rsl-django) $ mkdir django/
(rsl-django) $ cd django/
(rsl-django) $ django-admin startproject webgame
```

### Djangoアプリケーションの作成
```bash
(rsl-django) $ cd webgame/
(rsl-django) $ python manage.py startapp touch
```

## 仮想環境のディアクティベート
```bash
(rsl-django) $ deactivate
```

## PyCharmのインストール
```bash
sudo apt install snapd
sudo snap install pycharm-community --classic
```

## PyCharmでプロジェクトのオープン
```bash
pycharm-community
```

1. **Open**ボタンをクリックする。
   1. Djangoプロジェクトディレクトリ（下記の場所）を選択する。
      - `【ホームディレクトリ】/django/webgame`
      - ※`【ホームディレクトリ】`には各自のホームディレクトリ（`/home/***`）を指定する。
   2. **OK**ボタンをクリックする。

## Python Interpreterの設定
1. **File - Settings**を開く。
   1. **Project: webgame - Python Interpreter**を開く。
      1. **Python Interpreter**の右側の歯車アイコンをクリックし、**Add**を選択する。
         1. **Virtualenv Environment**を開き、下記を設定する。
            - **Existing environment**: 選択
            - **Interpreter**: `【ホームディレクトリ】/venv/rsl-django/bin/python3.9`
         2. **OK**ボタンをクリックする。
      2. **OK**ボタンをクリックする。

## runserverの登録
1. 右上の**Add Configuration**ボタンをクリックする。
   1. 左上の**+**ボタンをクリックし、**Python**を選択する。
      1. 下記を設定する。
         - **Name**: `runserver`
         - **Script path**: `【ホームディレクトリ】/django/webgame/manage.py`
         - **Parameters**: `runserver`
         - **Environment Variables**: 
           - **DB_USER**: 【DBユーザ名】
           - **DB_PASSWORD**: 【パスワード】
           - ※【DBユーザ名】と【パスワード】は、PostgreSQLにログインするためのユーザ名とパスワードを指定する。
      2. **OK**ボタンをクリックする。

## runserverの起動
1. 右上のプルダウンリストが`runserver`になっていることを確認し，**▶**ボタンをクリックする。
2. 下記にアクセスし、「The install worked successfully! Congratulations!」と表示されればOK。
   - http://127.0.0.1:8000/

## makemigrationsの登録
1. 右上のプルダウンリストから**Edit Configurations**を開く。
   1. 登録してある`runserver`を選択し、**Copy Configuration**ボタンをクリックする。
   2. 下記を設定する。
      - **Name**: `makemigrations`
      - **Parameters**: `makemigrations`
   3. **OK**ボタンをクリックする。

## makemigrationsの実行
1. 右上のプルダウンリストから`makemigrations`を選択し、**▶**ボタンをクリックする。

## migrateの登録
1. 右上のプルダウンリストから**Edit Configurations**を開く。
   1. 登録してある`runserver`を選択し、**Copy Configuration**ボタンをクリックする。
   2. 下記を設定する。
      - **Name**: `migrate`
      - **Parameters**: `migrate`
   3. **OK**ボタンをクリックする。

## migrateの実行
1. 右上のプルダウンリストから`migrate`を選択し、**▶**ボタンをクリックする。

## inspectdbの登録
1. 右上のプルダウンリストから**Edit Configurations**を開く。
   1. 登録してある`runserver`を選択し、**Copy Configuration**ボタンをクリックする。
   2. 下記を設定する。
      - **Name**: `inspectdb`
      - **Parameters**: `inspectdb`
   3. **OK**ボタンをクリックする。

## inspectdbの実行
1. 右上のプルダウンリストから`migrate`を選択し、**▶**ボタンをクリックする。


