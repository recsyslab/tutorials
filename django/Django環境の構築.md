# Django環境の構築

## ライブラリのインストール
```bash
sudo apt install libgdal-dev
```

## 仮想環境の複製
```bash
cd ~/venv/
source rsl-base/bin/activate
```

```bash
(rsl-base) $ pip freeze > rsl-base_requirements.txt
(rsl-base) $ deactivate
```

## rsl-django環境の構築

```bash
python3.9 -m venv rsl-django
source rsl-django/bin/activate
```

```bash
(rsl-django) $ pip install --upgrade pip
(rsl-django) $ pip install -r rsl-base_requirements.txt
(rsl-django) $ pip freeze
```

## django関連パッケージのインスト―ル
```bash
(rsl-django) $ pip install django
(rsl-django) $ pip install django-leaflet
(rsl-django) $ export CPLUS_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ export C_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ pip install gdal==3.0.4 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
(rsl-django) $ pip install djangorestframework-gis # RESTful APIに必要
(rsl-django) $ pip install django-filter # RESTful APIに必要
(rsl-django) $ pip install markdown # RESTful APIに必要
(rsl-django) $ pip install django-bootstrap4
(rsl-django) $ pip install django-allauth
(rsl-django) $ pip freeze
```

# Djangoプロジェクトの作成
```bash
(rsl-django) $ cd ~/recsyslab/
(rsl-django) $ mkdir django/
(rsl-django) $ cd django/
(rsl-django) $ django-admin startproject myproject
```

# Djangoアプリケーションの作成
```bash
(rsl-django) $ cd myproject/
(rsl-django) $ python manage.py startapp myrecsys
```
