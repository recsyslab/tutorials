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
pip freeze > rsl-base_requirements.txt
deactivate
```

## rsl-django環境の構築

```bash
python3.9 -m venv rsl-django
source rsl-django/bin/activate
```

```bash
pip install --upgrade pip
pip install -r rsl-base_requirements.txt
pip freeze
```

## django関連パッケージのインスト―ル
```bash
pip install django
pip install django-leaflet
export CPLUS_INCLUDE_PATH=/usr/include/gdal
export C_INCLUDE_PATH=/usr/include/gdal
pip install gdal==3.0.4 # libgdal-devのバージョンに合わせる # GeoDjangoに必要
pip install djangorestframework-gis # RESTful APIに必要
pip install django-filter # RESTful APIに必要
pip install markdown # RESTful APIに必要
pip install django-bootstrap4
pip install django-allauth
pip freeze
```

