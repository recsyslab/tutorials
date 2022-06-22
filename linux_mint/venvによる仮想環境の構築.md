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
python --version
python3 --version
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
cd ~/venv/
python3.9 -m venv rsl-base
source rsl-base/bin/activate
```

### pipのアップグレード
```bash
pip --version
pip install --upgrade pip
pip --version
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
pip install ipywidgets
```

### インストール済みパッケージ一覧の確認
```bash
pip freeze
```

### 実行テスト
(TBD)

### 各種パッケージのバージョンの確認
```bash
ipython --version
python
# プロンプトが>>>となればOK
```

```python
import numpy
numpy.__version__
import scipy
scipy.__version__
import matplotlib
matplotlib.__version__
import pandas
pandas.__version__
import sklearn
sklearn.__version__
import psycopg2
psycopg2.__version__
import tqdm
tqdm.__version__
[Ctrl+D]
```

### 仮想環境のディアクティベート
```bash
deactivate
```

## 仮想環境の複製

### 複製元の仮想環境からのインストール済みパッケージ情報の出力
```bash
cd ~/venv/
source rsl-base/bin/activate
```

```bash
pip freeze > rsl-base_requirements.txt
deactivate
```

### 複製先の仮想環境でのパッケージ情報の読込み
```bash
cd ~/venv/
python3.9 -m venv rsl-base-copy
source rsl-base-copy/bin/activate
```

```bash
pip install --upgrade pip
pip install -r rsl-base_requirements.txt
pip freeze
deactivate
```

#### 参考
- Zopfcode, [Pythonの環境管理ツール良し悪し](https://www.zopfco.de/entry/2017/04/03/233811)
- GitHub, pandas-dev/pandas, [import pandas error for missing compression libraries #27575](https://github.com/pandas-dev/pandas/issues/27575)
- stack overflow, [“UserWarning: Matplotlib is currently using agg, which is a non-GUI backend, so cannot show the figure.” when plotting figure with pyplot on Pycharm](https://stackoverflow.com/questions/56656777/userwarning-matplotlib-is-currently-using-agg-which-is-a-non-gui-backend-so)
- UYA ROOM, [【Python】venvで作成した仮想環境をコピーする方法](https://uyaroom.com/python-venv/)
