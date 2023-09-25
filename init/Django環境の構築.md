# Django環境の構築

## 仮想環境の複製

### rsl-base環境からのインストール済みパッケージ情報の出力
```bash
$ cd ~/venv/
$ source rsl-base/bin/activate
(rsl-base) $ pip freeze > rsl-base_requirements.txt
(rsl-base) $ deactivate
```

### rsl-django環境でのパッケージ情報の読込み
```bash
$ cd ~/venv/
$ python3.11 -m venv rsl-django
$ source rsl-django/bin/activate
(rsl-django) $ pip install --upgrade pip
(rsl-django) $ pip install -r rsl-base_requirements.txt
(rsl-django) $ pip freeze
(rsl-django) $ deactivate
```
