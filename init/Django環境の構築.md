# Django環境の構築

## 仮想環境の複製

### rsl-base環境からのインストール済みパッケージ情報の出力
```bash
$ source ~/venv/rsl-base/bin/activate
(rsl-base) $ pip freeze > ~/venv/rsl-base_requirements.txt
(rsl-base) $ deactivate
```

### venv_recsys_django環境でのパッケージ情報の読込み
```bash
$ python3.11 -m venv ~/venv_recsys_django
$ source ~/venv_recsys_django/bin/activate
(venv_recsys_django) $ pip install --upgrade pip
(venv_recsys_django) $ pip install -r ~/venv/rsl-base_requirements.txt
(venv_recsys_django) $ pip freeze
(venv_recsys_django) $ deactivate
```

## django関連パッケージのインスト―ル
```bash
$ source ~/venv/venv_recsys_django/bin/activate
(venv_recsys_django) $ pip install django
(venv_recsys_django) $ pip install django-leaflet
(venv_recsys_django) $ export CPLUS_INCLUDE_PATH=/usr/include/gdal
(venv_recsys_django) $ export C_INCLUDE_PATH=/usr/include/gdal
(venv_recsys_django) $ apt list --installed | grep libgdal-dev
libgdal-dev/jammy,now 3.4.1+dfsg-1build4 amd64 [インストール済み]
(venv_recsys_django) $ pip install gdal==3.4.1 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
(venv_recsys_django) $ pip install djangorestframework-gis # RESTful APIに必要
(venv_recsys_django) $ pip install django-filter # RESTful APIに必要
(venv_recsys_django) $ pip install markdown # RESTful APIに必要
(venv_recsys_django) $ pip install django-bootstrap5
(venv_recsys_django) $ pip install django-allauth
(venv_recsys_django) $ pip install django-cleanup
```

## インストール済みパッケージ一覧の確認と出力
```bash
(venv_recsys_django) $ pip freeze
(venv_recsys_django) $ pip freeze > ~/venv/venv_recsys_django_requirements.txt
(venv_recsys_django) $ deactivate
```

## Django環境の動作テスト

### Djangoプロジェクトの作成
```bash
(venv_recsys_django) $ cd
(venv_recsys_django) $ mkdir django-test/
(venv_recsys_django) $ cd django-test/
(venv_recsys_django) $ django-admin startproject test_project
(venv_recsys_django) $ ls
# `test_project/`が正しく生成されればOK
```

### Djangoアプリケーションの作成
```bash
(venv_recsys_django) $ cd test_project/
(venv_recsys_django) $ python manage.py startapp test_app
(venv_recsys_django) $ ls
# `test_app/`が正しく生成されればOK
```

### Djangoプロジェクトの削除
```bash
(venv_recsys_django) $ cd
(venv_recsys_django) $ rm -rf django-test/
(venv_recsys_django) $ deactivate
```
