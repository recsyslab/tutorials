# Jupyter

## Jupyter Notebookのインストール
```bash
sudo apt install jupyter-notebook
# ...（2分程度）...
```

## Jupyter Notebookの起動
```bash
jupyter-notebook
```

## Jupyterへのカーネルの追加 # rsl-base環境の場合
```bash
cd `/venv/
source rsl-base/bin/activate
```

```bash
pip install ipykernel
ipython kernel install --user --name=rsl-base --display-name=rsl-base
deactivate
```

## Jupyter Notebookでのカーネルの選択
```bash
jupyter-notebook
```

1. Jupyter Notebookで任意の`.ipynb`ファイルを開く。
2. メニューから**Kernel > Change kernel > rsl-base**を選択する。
3. `skl01_plt01.py`を実行する。
   - 図が表示されればOK。

## Jupyter Notebookでのtqdmの利用
プログレスバーの表示がおかしい場合は、下記の拡張機能を有効化してみる。
```bash
sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
```

#### 参考
- Qiita, [Ubuntu 18.04 LTS で Jupyter Notebook 環境構築](https://qiita.com/zono_0/items/49eb8605ef4d841b2c26)
- Qiita, [jupyter notebookでvenvを使う](https://qiita.com/Gattaca/items/80a5d36673ba2b6ef7f0)
- Qiita, [Jupyter Notebook でプログレスバーを出す](https://qiita.com/halhorn/items/e8aaf5b63f493f038a53)
