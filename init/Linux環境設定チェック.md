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


# Xドライブのマウント
sudo umount X_DRIVE
sudo mount -t vboxsf X_DRIVE /mnt/x/
df -h

```

```bash
$ ~/bin/startup.sh 
...（略）...
Filesystem      Size  Used Avail Use% Mounted on
...（略）...
/dev/sda3       314G   18G  280G   6% /
...（略）...
C_DRIVE         476G  446G   31G  94% /mnt/c
X_DRIVE         895G  878G   18G  99% /mnt/x
```

### ホームディレクトリの確認
```bash
$ ls -l
合計 48
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Desktop
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Documents
drwxr-xr-x 2 rsl rsl 4096  9月  9 19:02 Downloads
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Music
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Pictures
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Public
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Templates
drwxr-xr-x 2 rsl rsl 4096  9月  9 18:41 Videos
drwxrwxr-x 2 rsl rsl 4096  9月 10 13:01 bin
drwxrwxr-x 3 rsl rsl 4096  9月 10 10:11 opt
drwxrwxr-x 4 rsl rsl 4096  9月 10 10:42 src
drwxrwxr-x 3 rsl rsl 4096  9月 10 10:23 venv
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
2024年  9月 10日 火曜日 13:04:38 JST
```

### サービス一覧の確認
```bash
$ systemctl list-unit-files --type service
...（略）...
postgresql.service                           enabled         enabled
...（略）...
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
Description:	Linux Mint 22
Release:	22
Codename:	wilma
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
/dev/sda3       314G   18G  280G   6% /
...（略）...
C_DRIVE         476G  449G   28G  95% /mnt/c
X_DRIVE         895G  878G   18G  99% /mnt/x
$ cat /proc/cpuinfo | grep 'model name'
model name	: 12th Gen Intel(R) Core(TM) i7-12650H
...（略）...
$ cat /proc/cpuinfo | grep processor
...（略）...
processor	: 3
$ cat /proc/meminfo | grep MemTotal
MemTotal:       11087264 kB
```

#### 参考
- クロの思考ノート, [Linuxのシステムやハードウェア情報を取得するコマンドを集めてみた](http://note.kurodigi.com/linux-systeminfo/)
- Qiita, [ifconfigの出力結果に書いてあること #Linux - Qiita](https://qiita.com/pe-ta/items/aff8db72530c6baa11b2)

## Google Crhome

### バージョンの確認
```bash
$ google-chrome --version
Google Chrome 128.0.6613.119 
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
---------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 16.4 (Ubuntu 16.4-0ubuntu0.24.04.2) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 13.2.0-23ubuntu4) 13.2.0, 64-bit
(1 row)

postgres=# \q
```

### PostgreSQLサーバの起動確認
```bash
$ systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; preset: enabled)
     Active: active (exited) since Tue 2024-09-10 12:59:58 JST; 17min ago
    Process: 1903 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 1903 (code=exited, status=0/SUCCESS)
        CPU: 1ms
...（略）...
``

### 設定ファイルの確認
```bash
$ sudo diff /etc/postgresql/16/main/pg_hba.conf-org /etc/postgresql/16/main/pg_hba.conf
118c118
< local   all             postgres                                peer
---
> local   all             postgres                                md5
123c123
< local   all             all                                     peer
---
> local   all             all                                     md5
130c130
< local   replication     all                                     peer
---
> local   replication     all                                     md5
```

```bash
$ sudo diff /etc/postgresql/16/main/postgresql.conf-org /etc/postgresql/16/main/postgresql.conf
620c620
< #track_counts = on
---
> track_counts = on
640c640
< #autovacuum = on			# Enable autovacuum subprocess?  'on'
---
> autovacuum = on			# Enable autovacuum subprocess?  'on'
```

## Python

### バージョンの確認
```bash
$ python3 --version
Python 3.12.3
$ python3.12 --version
Python 3.12.6
```

### 仮想環境の確認
```bash
$ cd
$ source ~/venv/rsl-base/bin/activate
(rsl-base) $ pip --version
pip 24.2 from /home/rsl/venv/rsl-base/lib/python3.12/site-packages/pip (python 3.12)
(rsl-base) $ pip freeze
# （一部抜粋）
importlib==1.0.4
importnb==2023.11.1
ipython==8.27.0
matplotlib==3.9.2
matplotlib-inline==0.1.7
numpy==2.1.1
pandas==2.2.2
pillow==10.4.0
psycopg2-binary==2.9.9
requests==2.32.3
scikit-learn==1.5.1
scipy==1.14.1
timedelta==2020.12.3
tqdm==4.66.5
(rsl-base) $ deactivate
$
```

## Visual Studio Code
```bash
$ code -v
1.93.0
4849ca9bdf9666755eb463db297b69e5385090e3
x64
```

## Linux Mintの修了
```bash
$ shutdown -h now
```

## 仮想マシンのスナップショットの作成
1. 対象の仮想マシンが**電源オフ**になっていることを確認する。
2. 対象の仮想マシンを選択し、**作成**ボタンをクリックする。
   1. 下記を設定し、**OK**ボタンをクリックする。
      - **スナップショットの名前**: `初期セットアップ完了`

## 仮想マシンのクローン
1. 作成したスナップショットを選択し、**クローン**ボタンをクリックする。
   1. 下記を設定し、**次へ**ボタンをクリックする。
      - **名前**: `Linux Mint 22 MATE 64-bit | initial`
      - **パス**: `Z:\VirtualBox VMs`
   2. **すべてをクローン**を選択し、**次へ**ボタンをクリックする。
   3. **現在のマシンの状態**を選択し、**完了**ボタンをクリックする。
      - ...（15分程度）...14:30
