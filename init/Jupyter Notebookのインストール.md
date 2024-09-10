# Jupyter Notebookのインストール

## インストール
```bash
$ sudo apt install jupyter-notebook
# ...（2分程度）...
```

## バージョンの確認
```bash
$ jupyter-notebook --version
6.4.12
```

## Jupyterへのカーネルの追加
```bash
$ source ~/venv/rsl-base/bin/activate
(rsl-base) $ pip install ipykernel
(rsl-base) $ ipython kernel install --user --name=rsl-base --display-name=rsl-base
(rsl-base) $ deactivate
```

## 起動
```bash
$ jupyter-notebook
```

## 動作テスト
1. **新規 > フォルダ**をクリックする。
2. 作成された`Untitled Folder`を選択し、**リネーム**ボタンをクリックする。
   1. 下記を設定し、**Rename**ボタンをクリックする。
    　- **Enter a new directory name:**: `data`
3. `data`フォルダに[`curry.csv`](../data/curry.csv)を置く。
4. **新規 > rsl-base**をクリックする。
5. 下記のソースコードを貼り付け、実行する。
   - 図が表示されればOK。

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
from sklearn.neighbors import KNeighborsClassifier


# data
curry = pd.read_csv('data/curry.csv', index_col=0)

feature_names = np.array(curry.columns[:-1])
target_names = ['dislike', 'neutral', 'like']

curry_X = np.array(curry[feature_names])
curry_y = np.array(curry['rating'])


# train
knn = KNeighborsClassifier()
knn.fit(curry_X, curry_y)


# plot
cmap_light = ListedColormap(['#CCCCFF', '#CCFFCC', '#FFCCCC'])
cmap_dark = ListedColormap(['#8888FF', '#88FF88', '#FF8888'])

x_min = 0
x_max = 100
y_min = 0
y_max = 100

xx, yy = np.mgrid[x_min:x_max:200j, y_min:y_max:200j]
Z = knn.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

plt.pcolormesh(xx, yy, Z, cmap=cmap_light)
plt.scatter(curry_X[:, 0], curry_X[:, 1], c=curry_y, cmap=cmap_dark, edgecolors='k')

plt.title("curry")
plt.xlabel('spicy')
plt.ylabel('thickness')
plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())

plt.show()
```

## Jupyter Notebookでのtqdmの利用
プログレスバーの表示がおかしい場合は、下記の拡張機能を有効化してみる。
```bash
$ sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
```

#### 参考
- Qiita, [Ubuntu 18.04 LTS で Jupyter Notebook 環境構築](https://qiita.com/zono_0/items/49eb8605ef4d841b2c26)
- Qiita, [jupyter notebookでvenvを使う](https://qiita.com/Gattaca/items/80a5d36673ba2b6ef7f0)
- Qiita, [Jupyter Notebook でプログレスバーを出す](https://qiita.com/halhorn/items/e8aaf5b63f493f038a53)
