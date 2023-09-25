# Django環境の構築

## 仮想環境の複製

### rsl-base環境からのインストール済みパッケージ情報の出力
```bash
$ source ~/venv/rsl-base/bin/activate
(rsl-base) $ pip freeze > rsl-base_requirements.txt
(rsl-base) $ deactivate
```

### rsl-django環境でのパッケージ情報の読込み
```bash
$ cd ~/venv/
$ python3.11 -m venv rsl-django
$ source ~/venv/rsl-django/bin/activate
(rsl-django) $ pip install --upgrade pip
(rsl-django) $ pip install -r rsl-base_requirements.txt
(rsl-django) $ pip freeze
(rsl-django) $ deactivate
```

## django関連パッケージのインスト―ル
```bash
$ source ~/venv/rsl-django/bin/activate
(rsl-django) $ pip install django
(rsl-django) $ pip install django-leaflet
(rsl-django) $ export CPLUS_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ export C_INCLUDE_PATH=/usr/include/gdal
(rsl-django) $ apt list --installed | grep libgdal-dev
libgdal-dev/jammy,now 3.4.1+dfsg-1build4 amd64 [インストール済み]
(rsl-django) $ pip install gdal==3.4.1 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
(rsl-django) $ pip install djangorestframework-gis # RESTful APIに必要
(rsl-django) $ pip install django-filter # RESTful APIに必要
(rsl-django) $ pip install markdown # RESTful APIに必要
(rsl-django) $ pip install django-bootstrap5
(rsl-django) $ pip install django-allauth
(rsl-django) $ pip install django-cleanup
```

## インストール済みパッケージ一覧の確認
```bash
(rsl-django) $ pip freeze
```
