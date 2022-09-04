# Django環境の構築

## 仮想環境の構築

### ライブラリのインストール
```bash
sudo apt install libgdal-dev
```

### 仮想環境の構築とアクティベート
```bash
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
pip install ipython
pip install numpy
pip install scipy
pip install matplotlib
pip install pandas
pip install scikit-learn
pip install psycopg2-binary
pip install tqdm
pip install mecab-python3
pip install requests
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

## PyCharmでプロジェクトのオープン
```bash
pycharm
```

1. **Open**ボタンをクリックする。
   1. Djangoプロジェクトディレクトリ（下記の場所）を選択する。
      - `【ホームディレクトリ】/django/webgame`
      - ※`【ホームディレクトリ】`には各自のホームディレクトリ（`/home/***`）を指定する。
   2. **OK**ボタンをクリックする。

## Python Interpreterの設定
1. **File - Settings**を開く。
   1. **Project: myproject - Python Interpreter**を開く。
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


