# PythonでMeCab

## 仮想環境のアクティベート
```bash
source rsl-base/bin/activate
```

## PythonでのMeCabのインポート
```bash
python
```

```python
import MeCab
# エラーが出なければOK。
```

## サンプルコードの実行
```
nano mecab_sample.py
# 参考サイト（https://qiita.com/ekzemplaro/items/c98c7f6698f130b55d53）のmecab_sample.pyのコードを作成してみる。
python mecab_sample.py
```

#### 参考
- Qiita, [anacondaのjupyter notebookからmecabを使うまでの手順](https://qiita.com/Kaisyou/items/293b0a0f4ee330bb4e49)
- Qiita, [mecab + NEologd + python3 で形態素解析](https://qiita.com/sudo5in5k/items/f89d9dc1bec1ed221ede)
- Qiita, [mecab-python3(0.996.1d)のpip installでErrorとその対応方法(google colabの場合)](https://qiita.com/siraasagi/items/e07e0b271cb7cd679a70)
- min117の日記, [fedora30 python3 から MeCabを使う → エラー「 [ifs] no such file or directory: /usr/local/etc/mecabrc」が出たら sudo cp /etc/mecabrc /usr/local/etc/ で解決](http://min117.hatenablog.com/entry/2020/07/11/145738)
