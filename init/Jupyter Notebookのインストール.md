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
3. `data`フォルダに[`curry.csv`](../../data/curry.csv)を置く。
1. **新規**ボタンのプルダウンメニューから**rsl-base**を選択する。
2. `data/`ディレクトリを作成し、
3. `skl01_plt01.py`を実行する。
   - 図が表示されればOK。

## Jupyter Notebookでのtqdmの利用
プログレスバーの表示がおかしい場合は、下記の拡張機能を有効化してみる。
```bash
$ sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
```

#### 参考
- Qiita, [Ubuntu 18.04 LTS で Jupyter Notebook 環境構築](https://qiita.com/zono_0/items/49eb8605ef4d841b2c26)
- Qiita, [jupyter notebookでvenvを使う](https://qiita.com/Gattaca/items/80a5d36673ba2b6ef7f0)
- Qiita, [Jupyter Notebook でプログレスバーを出す](https://qiita.com/halhorn/items/e8aaf5b63f493f038a53)
