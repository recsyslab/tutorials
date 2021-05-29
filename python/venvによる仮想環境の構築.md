# venvによる仮想環境の構築

## 基本操作

### 仮想環境の構築
```bash
mkdir ~/venv/
cd ~/venv/
python3.9 -m venv test
```

### 仮想環境のアクティベート
```bash
cd ~/venv/
source test/bin/activate
# プロンプトが(test) ...$となればOK
```

### 仮想環境内のPythonのバージョンの確認
```bash
(test) $ python --version
(test) $ python3 --version
```

### 仮想環境のディアクティベート
```bash
(test) $ deactivate
# プロンプトが元に戻ればOK
```

### 仮想環境の削除
```bash
rm -rf ~/venv/test/
```

## ベースとなる仮想環境

### 仮想環境の構築とアクティベート
```bash
$ cd ~/venv/
$ python3.9 -m venv rsl-base
$ source rsl-base/bin/activate
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
```

### インストール済みパッケージ一覧の確認
```bash
(rsl-base) $ pip freeze
```

### 実行テスト
scikit-learnチュートリアルのskl01_plt01.pyとdata/curry.csvを~/venv/ディレクトリに置き，下記を実行する．画像が表示されればOK．

1
(base) $ python skl01_plt01.py
各種パッケージのバージョンの確認
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
(base) $ ipython --version
(base) $ python
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
[Ctrl+D]
ディアクティベート
1
(base) $ deactivate
仮想環境の複製
複製元の仮想環境からのインストール済みパッケージ情報の出力
1
2
3
4
$ cd ~/venv/
$ source base/bin/activate
(base) $ pip freeze > requirements.txt
(base) $ deactivate
複製先の仮想環境でのパッケージ情報の読込み
1
2
3
4
5
6
7
$ cd ~/venv/
$ python3.8 -m venv base-copy
$ source base-copy/bin/activate
(base-copy) $ pip install --upgrade pip
(base-copy) $ pip install -r requirements.txt
(base-copy) $ pip freeze
(base-copy) $ deactivate
参考
GitHub, pandas-dev/pandas, import pandas error for missing compression libraries #27575
stack overflow, “UserWarning: Matplotlib is currently using agg, which is a non-GUI backend, so cannot show the figure.” when plotting figure with pyplot on Pycharm
UYA ROOM, 【Python】venvで作成した仮想環境をコピーする方法

#### 参考
- Zopfcode, [Pythonの環境管理ツール良し悪し](https://www.zopfco.de/entry/2017/04/03/233811)
