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
deactivate
# プロンプトが元に戻ればOK
```

### 仮想環境の削除
```bash
rm -rf ~/venv/test/
```

#### 参考
- Zopfcode, [Pythonの環境管理ツール良し悪し](https://www.zopfco.de/entry/2017/04/03/233811)
