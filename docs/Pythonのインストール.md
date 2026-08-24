# Pythonのインストール

## 事前準備

```bash
$ sudo apt install libbz2-dev       # pandasのインポートに必要
$ sudo apt install python3-tk       # matplotlib.show()で画像を表示する際に必要
$ sudo apt install build-essential  # GDALのインストールに必要
$ sudo apt install libgdal-dev      # GDALのインストールに必要
# ...（1分程度）...
$ sudo apt install python3-gdal	    # GDALのインストールに必要
$ sudo apt install libffi-dev       # scikit-learnのインポートに必要
```

## インストール

```bash
$ mkdir -p ~/opt/python/
$ cd ~/src/
$ wget https://www.python.org/ftp/python/3.12.6/Python-3.12.6.tar.xz
$ xz -dc Python-3.12.6.tar.xz| tar xfv -
$ cd Python-3.12.6/
$ ./configure --prefix=$HOME/opt/python --with-ensurepip=install
# ...（3分程度）...
$ make 2>&1 | tee make.log
# ...（3分程度）...
$ make altinstall
# ...（2分程度）...
$ rm -f ~/src/Python-3.12.6.tar.xz
```

## インストール結果の確認

```bash
$ ls ~/opt/python/
bin  include  lib  share
$ ls ~/opt/python/bin/ -alh
合計 31M
drwxr-xr-x 2 rsl rsl 4.0K  9月 10 10:19 .
drwxrwxr-x 6 rsl rsl 4.0K  9月 10 10:19 ..
-rwxr-xr-x 1 rsl rsl  112  9月 10 10:19 2to3-3.12
-rwxr-xr-x 1 rsl rsl  110  9月 10 10:19 idle3.12
-rwxrwxr-x 1 rsl rsl  240  9月 10 10:19 pip3.12
-rwxr-xr-x 1 rsl rsl   95  9月 10 10:19 pydoc3.12
-rwxr-xr-x 1 rsl rsl  31M  9月 10 10:18 python3.12
-rwxr-xr-x 1 rsl rsl 3.0K  9月 10 10:19 python3.12-config
```

## パスの設定

```bash
$ echo $PATH
/home/rsl/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ less ~/.profile
$
 echo -e '\n# Pythonインストール時に追加' >> ~/.profile
 echo 'export PATH="$HOME/opt/python/bin:$PATH"' >> ~/.profile
$
 less ~/.profile
 diff ~/.profile-org ~/.profile
27a28,33
>
>
> #### #### Add below. #### ####
>
> # Pythonインストール時に追加
> export PATH="$HOME/opt/python/bin:$PATH"

$ echo $PATH
/home/rsl/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ source ~/.profile
$ echo $PATH
/home/rsl/opt/python/bin:/home/rsl/bin:/home/rsl/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

## バージョンの確認

```bash
$ python3 --version
Python 3.12.3
$ python3.12 --version
Python 3.12.6
```

## ベースとなる仮想環境

### 仮想環境の構築とアクティベート

```bash
$ cd
$ mkdir ~/venv/
$ python3.12 -m venv ~/venv/rsl_base
$ source ~/venv/rsl_base/bin/activate
(rsl_base) $
```

仮想環境にアクティベートすると、プロンプトの先頭に`(rsl_base)`のように仮想環境名が表示される。以降、`(<VENV>) $`と記載している箇所は、仮想環境`<VENV>`にアクティベートした状態で入力するコマンドを表す。

### pipのアップグレード

```bash
(rsl_base) $ pip --version
(rsl_base) $ pip install --upgrade pip
(rsl_base) $ pip --version
pip 26.2.1 from /home/rsl/venv/rsl_base/lib/python3.12/site-packages/pip (python 3.12)
```

### 各種パッケージのインストール

```bash
(rsl_base) $
 pip install ipython

 pip install numpy
 pip install scipy
 pip install matplotlib
 pip install pandas
 pip install scikit-learn
 pip install hdbscan

 pip install psycopg2-binary

 pip install tqdm
 pip install requests
 pip install importnb
 pip install importlib
 pip install beautifulsoup4
 pip install cryptography

 pip install mecab-python3
 pip install spacy
 python -m spacy download en_core_web_sm
 pip install ginza
 pip install ja-ginza
 pip install nltk

 pip install opencv-python

 pip install openai

 pip install django
 pip install django-filter
 pip install django-cors-headers
 pip install djangorestframework
 pip install djangorestframework-simplejwt

 pip install gunicorn
```

### インストール済みパッケージ一覧の確認

```bash
(rsl_base) $ pip freeze
# ipython
asttokens==3.0.1
decorator==5.2.1
executing==2.2.1
ipython==9.13.0
ipython_pygments_lexers==1.1.1
jedi==0.19.2
matplotlib-inline==0.2.1
parso==0.8.6
pexpect==4.9.0
prompt_toolkit==3.0.52
psutil==7.2.2
ptyprocess==0.7.0
pure_eval==0.2.3
Pygments==2.20.0
stack-data==0.6.3
traitlets==5.14.3
wcwidth==0.6.0

# numpy
numpy==2.4.4

# scipy
scipy==1.17.1

# matplotlib
contourpy==1.3.3
cycler==0.12.1
fonttools==4.62.1
kiwisolver==1.5.0
matplotlib==3.10.9
packaging==26.2
pillow==12.2.0
pyparsing==3.3.2
python-dateutil==2.9.0.post0
six==1.17.0

# pandas
pandas==3.0.2

# scikit-learn
joblib==1.5.3
scikit-learn==1.8.0
threadpoolctl==3.6.0

# hdbscan
hdbscan==0.8.42

# psycopg2-binary
psycopg2-binary==2.9.12

# tqdm
tqdm==4.67.3

# requests
certifi==2026.4.22
charset-normalizer==3.4.7
idna==3.13
requests==2.33.1
urllib3==2.6.3

# importnb
importnb==2023.11.1

# importlib
importlib==1.0.4

# beautifulsoup4
beautifulsoup4==4.14.3
soupsieve==2.8.3

# cryptgraphy
cffi==2.0.0
cryptography==47.0.0
pycparser==3.0

# mecab-python3
mecab-python3==1.0.12

# spacy
annotated-doc==0.0.4
annotated-types==0.7.0
anyio==4.13.0
blis==1.3.3
catalogue==2.0.10
click==8.3.3
cloudpathlib==0.23.0
confection==1.3.3
cymem==2.0.13
h11==0.16.0
httpcore==1.0.9
httpx==0.28.1
Jinja2==3.1.6
markdown-it-py==4.0.0
MarkupSafe==3.0.3
mdurl==0.1.2
murmurhash==1.0.15
preshed==3.0.13
pydantic==2.13.3
pydantic_core==2.46.3
rich==15.0.0
setuptools==82.0.1
shellingham==1.5.4
smart_open==7.6.0
spacy==3.8.14
spacy-legacy==3.0.12
spacy-loggers==1.0.5
srsly==2.5.3
thinc==8.3.13
typer==0.24.2
typing-inspection==0.4.2
wasabi==1.1.3
weasel==1.0.0
wrapt==2.1.2

# en_core_web_sm
en_core_web_sm @ https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl#sha256=1932429db727d4bff3deed6b34cfc05df17794f4a52eeb26cf8928f7c1a0fb85

# ginza
ginza==5.2.0
plac==1.4.5
SudachiDict-core==20260116
SudachiPy==0.6.11

# ja-ginza
ja-ginza==5.2.0

# nltk
nltk==3.9.4
regex==2026.4.4

# opencv-python
opencv-python==4.13.0.92

# openai
distro==1.9.0
jiter==0.14.0
openai==2.32.0
sniffio==1.3.1

# django
asgiref==3.11.1
Django==6.0.4
sqlparse==0.5.5

# django-filter
django-filter==25.2

# django-cors-headers
django-cors-headers==4.9.0

# djangorestframework
djangorestframework==3.17.1

# djangorestframework_simplejwt
djangorestframework_simplejwt==5.5.1
PyJWT==2.12.1

# gunicorn
gunicorn==25.3.0
```

### インストール済みパッケージ情報の出力

```bash
(rsl_base) $ pip freeze > ~/venv/requirements_rsl_base.txt
```

### 仮想環境のディアクティベート

```bash
(rsl_base) $ deactivate
$
```

仮想環境をディアクティベートすると、プロンプトが元に戻る。

## PyTorchを利用する仮想環境

### 仮想環境の複製

```bash
$ cd
$ python3.12 -m venv ~/venv/rsl_base_torch
$ source ~/venv/rsl_base_torch/bin/activate
(rsl_base_torch) $ pip install --upgrade pip
(rsl_base_torch) $ pip --version
(rsl_base_torch) $ pip install -r ~/venv/requirements_rsl_base.txt
(rsl_base_torch) $ pip freeze
```

### 各種パッケージのインストール

```bash
(rsl_base_torch) $
 pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
 # ...（3分程度）...
 pip install torchinfo
 pip install ray
 pip install pyarrow
 pip install kmeans_pytorch
 pip install recbole

 pip install transformers["ja"]
 pip install sentence-transformers
```

### インストール済みパッケージ一覧の確認

```bash
(rsl_base_torch) $ pip freeze
# torch, torchvision, torchaudio
filelock==3.25.2
fsspec==2026.2.0
mpmath==1.3.0
networkx==3.6.1
nvidia-cublas-cu12==12.4.5.8
nvidia-cuda-cupti-cu12==12.4.127
nvidia-cuda-nvrtc-cu12==12.4.127
nvidia-cuda-runtime-cu12==12.4.127
nvidia-cudnn-cu12==9.1.0.70
nvidia-cufft-cu12==11.2.1.3
nvidia-curand-cu12==10.3.5.147
nvidia-cusolver-cu12==11.6.1.9
nvidia-cusparse-cu12==12.3.1.170
nvidia-cusparselt-cu12==0.6.2
nvidia-nccl-cu12==2.21.5
nvidia-nvjitlink-cu12==12.4.127
nvidia-nvtx-cu12==12.4.127
sympy==1.13.1
torch==2.6.0+cu124
torchaudio==2.6.0+cu124
torchvision==0.21.0+cu124
triton==3.2.0

# torchinfo
torchinfo==1.8.0

# ray
attrs==26.1.0
jsonschema==4.26.0
jsonschema-specifications==2025.9.1
msgpack==1.1.2
protobuf==7.34.1
PyYAML==6.0.3
ray==2.54.1
referencing==0.37.0
rpds-py==0.30.0

# pyarror
pyarrow==23.0.1

# kmeans-pytorch
kmeans-pytorch==0.3

# recbole
absl-py==2.4.0
colorama==0.4.4
colorlog==4.7.2
grpcio==1.80.0
Markdown==3.10.2
narwhals==2.19.0
plotly==6.7.0
recbole==1.2.0
tabulate==0.10.0
tensorboard==2.20.0
tensorboard-data-server==0.7.2
texttable==1.7.0
thop==0.1.1.post2209072238
Werkzeug==3.1.8

# transformers["ja"]
fugashi==1.5.2
hf-xet==1.4.3
huggingface_hub==1.10.1
ipadic==1.0.0
regex==2026.4.4
rhoknp==1.3.0
safetensors==0.7.0
tokenizers==0.22.2
transformers==5.5.3
unidic==1.1.0
unidic-lite==1.0.8
< wasabi==1.1.3
> wasabi==0.10.1

# sentence-transformers
sentence-transformers==5.4.0
```

### インストール済みパッケージ情報の出力

```bash
(rsl_base_torch) $ pip freeze > ~/venv/requirements_rsl_base_torch.txt
```

### 仮想環境のディアクティベート

```bash
(rsl_base_torch) $ deactivate
$
```

### 仮想環境のサイズの確認

```bash
$ du -sh ~/venv/*/
1.4G	/home/rsl/venv/rsl_base/
7.3G	/home/rsl/venv/rsl_base_torch/
```

#### 参考

1. Doitu.info, [既存のPython環境を壊すことなく、自分でビルドしてインストールする（altinstall）](https://doitu.info/blog/5c45e5ec8dbc7a001af33ce8)
1. 組み込みの人。, [makeコマンドのちょっとしたtips](https://embedded.hatenadiary.org/entry/20090416/p1)
1. Zopfcode, [Pythonの環境管理ツール良し悪し](https://www.zopfco.de/entry/2017/04/03/233811)
1. GitHub, pandas-dev/pandas, [import pandas error for missing compression libraries #27575](https://github.com/pandas-dev/pandas/issues/27575)
1. stack overflow, [“UserWarning: Matplotlib is currently using agg, which is a non-GUI backend, so cannot show the figure.” when plotting figure with pyplot on Pycharm](https://stackoverflow.com/questions/56656777/userwarning-matplotlib-is-currently-using-agg-which-is-a-non-gui-backend-so)
1. UYA ROOM, [【Python】venvで作成した仮想環境をコピーする方法](https://uyaroom.com/python-venv/)
