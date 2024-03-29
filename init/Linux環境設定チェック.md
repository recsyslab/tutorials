# Linux環境設定チェック

## 初期設定の確認

### パッケージのアップグレード
```bash
$ sudo apt update
$ sudo apt upgrade
```

### スタートアップシェルスクリプトの実行
```bash
$ less ~/bin/startup.sh
```

```bash
#! /bin/sh


# Cドライブのマウント
sudo umount C_DRIVE
sudo mount -t vboxsf C_DRIVE /mnt/c/
df -h


# Dドライブのマウント <- Dドライブ（外付けSSD）をマウントした場合
sudo umount D_DRIVE
sudo mount -t vboxsf D_DRIVE /mnt/d/
df -h


# PostgreSQLサーバの起動
sudo service postgresql start
```

```bash
$ ~/bin/startup.sh 
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           1.1G  1.3M  1.1G   1% /run
/dev/sda3       314G   18G  281G   6% /
tmpfs           5.3G   28K  5.3G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
/dev/sda2       512M  6.1M  506M   2% /boot/efi
tmpfs           1.1G  120K  1.1G   1% /run/user/1000
C_DRIVE         476G  417G   60G  88% /mnt/c
D_DRIVE         477G   89G  389G  19% /mnt/d    # <- Dドライブ（外付けSSD）をマウントした場合
```

### ホームディレクトリの確認
```bash
$ ls -l
合計 48
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Desktop
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Documents
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:48 Downloads
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Music
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Pictures
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Public
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Templates
drwxr-xr-x 2 rsl rsl 4096  9月  2 13:14 Videos
drwxrwxr-x 2 rsl rsl 4096  9月  2 13:20 bin
drwxrwxr-x 3 rsl rsl 4096  9月 25 12:09 opt
drwxrwxr-x 5 rsl rsl 4096  9月 25 12:15 src
drwxrwxr-x 5 rsl rsl 4096  9月 29 13:17 venv
```

### 設定ファイルの確認
```bash
$ diff ~/.profile-org ~/.profile
27a28,33
> 
> 
> #### #### Add below. #### ####
> 
> # Pythonインストール時に追加
> export PATH="$HOME/opt/python/bin:$PATH"
```

```bash
$ diff ~/.bashrc-org ~/.bashrc
117a118,120
> 
> 
> #### #### Add below. #### ####
```

```bash
$ diff /etc/apt/sources.list-org /etc/apt/sources.list
7a8,10
> 
> 
> #### #### Add below. #### ####
```

### 時刻の確認
```bash
$ date
yyyy年 mm月 dd日 月曜日 HH:MM:SS JST
```

### sysv-rc-confのバージョンの確認
```bash
$ sudo sysv-rc-conf
# [h]キーでバージョンを確認できる。
# [q]キーで終了する。
```

### ファイアウォールの確認
```bash
$ sudo ufw status
状態: アクティブ
```

## システム情報等の確認

### ディストリビューションバージョンの確認
```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Linuxmint
Description:	Linux Mint 21.2
Release:	21.2
Codename:	victoria
```

### アーキテクチャの確認
```bash
$ arch
x86_64
```

### ホスト名の確認
```bash
$ hostname
recsyslab-mint
```

### IPアドレスの確認
```bash
$ ifconfig
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
...（略）...
```

### スペック（ディスクサイズ，CPU，メモリ）の確認
```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
...（略）...
/dev/sda3       314G   19G  280G   7% /
...（略）...
C_DRIVE         477G  403G   74G  85% /mnt/c
D_DRIVE         477G   92G  386G  20% /mnt/d
$ cat /proc/cpuinfo | grep 'model name'
model name	: 12th Gen Intel(R) Core(TM) i7-12650H
...（略）...
$ cat /proc/cpuinfo | grep processor
...（略）...
processor	: 9
$ cat /proc/meminfo | grep MemTotal
MemTotal:       11098216 kB
```

#### 参考
- クロの思考ノート, [Linuxのシステムやハードウェア情報を取得するコマンドを集めてみた](http://note.kurodigi.com/linux-systeminfo/)
- Qiita, [ifconfigの出力結果に書いてあること #Linux - Qiita](https://qiita.com/pe-ta/items/aff8db72530c6baa11b2)

## Google Crhome

### バージョンの確認
```bash
$ google-chrome --version
Google Chrome 118.0.5993.88
```

## Symantec Endpoint Protection (SEP)

### バージョンの確認
```bash
$ /opt/Symantec/symantec_antivirus/sav info --product
14.3 (14.3 MP1) build 1148 (14.3.1148.0100)
```

## PostgreSQL+PostGIS

### バージョンの確認
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# SELECT version();
                                                               version                                                                
--------------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 14.9 (Ubuntu 14.9-0ubuntu0.22.04.1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0, 64-bit
(1 row)
postgres=# \q
```

### PostgreSQLサーバの起動確認
```bash
$ service postgresql status
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor pr>
     Active: active (exited) since Mon 2023-10-23 22:25:34 JST; 1h 4min ago
   Main PID: 1894 (code=exited, status=0/SUCCESS)
        CPU: 2ms
...（略）...
``

### 設定ファイルの確認
```bash
$ sudo diff /etc/postgresql/14/main/pg_hba.conf-org /etc/postgresql/14/main/pg_hba.conf
90c90
< local   all             postgres                                peer
---
> local   all             postgres                                md5
95c95
< local   all             all                                     peer
---
> local   all             all                                     md5
102c102
< local   replication     all                                     peer
---
> local   replication     all                                     md5
```

```bash
$ sudo diff /etc/postgresql/14/main/postgresql.conf-org /etc/postgresql/14/main/postgresql.conf
600c600
< #track_counts = on
---
> track_counts = on
620c620
< #autovacuum = on			# Enable autovacuum subprocess?  'on'
---
> autovacuum = on			# Enable autovacuum subprocess?  'on'
```

## Python

### バージョンの確認
```bash
$ python3 --version
Python 3.10.12
$ python3.10 --version
Python 3.10.12
$ python3.11 --version
Python 3.11.5
```

### 仮想環境の確認
```bash
$ cd
$ source ~/venv/rsl-base/bin/activate
(rsl-base) $ pip --version
pip 23.3.1 from /home/rsl/venv/rsl-base/lib/python3.11/site-packages/pip (python 3.11)
(rsl-base) $ pip freeze
# （一部抜粋）
importlib==1.0.4
importnb==2023.1.7
ipython==8.15.0
matplotlib==3.8.0
mecab-python3==1.0.8
numpy==1.26.0
pandas==2.1.1
Pillow==10.0.1
psycopg2-binary==2.9.7
requests==2.31.0
scikit-learn==1.3.1
scipy==1.11.2
tqdm==4.66.1
(rsl-base) $ deactivate
```

## Visual Studio Code
```bash
$ code -v
1.83.1
f1b07bd25dfad64b0167beb15359ae573aecd2cc
x64
```




