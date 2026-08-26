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
/dev/sda3       503G   32G  445G   7% /
...（略）...
C_DRIVE         476G  442G   35G  93% /mnt/c
X_DRIVE         932G   55G  877G   6% /mnt/x
```

### ホームディレクトリの確認
```bash
$ ls -l
合計 48
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Desktop
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Documents
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Downloads
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Music
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Pictures
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Public
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Templates
drwxr-xr-x 2 rsl rsl 4096  8月 24 15:34 Videos
drwxrwxr-x 2 rsl rsl 4096  8月 24 14:17 bin
drwxrwxr-x 3 rsl rsl 4096  8月 24 14:58 opt
drwxrwxr-x 3 rsl rsl 4096  8月 26 14:32 src
drwxrwxr-x 4 rsl rsl 4096  8月 24 15:24 venv
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
2026年  8月 26日 水曜日 15:08:03 JST
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
Description:	Linux Mint 22.3
Release:	22.3
Codename:	zena
```

### アーキテクチャの確認
```bash
$ arch
x86_64
```

### ホスト名の確認
```bash
$ hostname
recXXX-mint
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
/dev/sda3       503G   32G  445G   7% /
...（略）...
C_DRIVE         476G  442G   35G  93% /mnt/c
X_DRIVE         932G   55G  877G   6% /mnt/x

$ cat /proc/cpuinfo | grep 'model name'
model name	: 12th Gen Intel(R) Core(TM) i7-12650H
...（略）...

$ cat /proc/cpuinfo | grep processor
processor	: 0
processor	: 1

$ cat /proc/meminfo | grep MemTotal
MemTotal:       11087264 kB
```

#### 参考
1. クロの思考ノート, [Linuxのシステムやハードウェア情報を取得するコマンドを集めてみた](http://note.kurodigi.com/linux-systeminfo/)
1. Qiita, [ifconfigの出力結果に書いてあること #Linux - Qiita](https://qiita.com/pe-ta/items/aff8db72530c6baa11b2)

## Google Crhome

### バージョンの確認
```bash
$ google-chrome --version
Google Chrome 152.0.7977.64 
```

## PostgreSQL+PostGIS

### バージョンの確認
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# SELECT version();
                                                                 version                                                                  
------------------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 16.15 (Ubuntu 16.15-0ubuntu0.24.04.1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 13.3.0-6ubuntu2~24.04.1) 13.3.0, 64-bit
(1 row)

postgres=# \q
```

### PostgreSQLサーバの起動確認
```bash
$ systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; prese>
     Active: active (exited) since Wed 2026-08-26 13:44:30 JST; 1h 30min ago
    Process: 1582 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 1582 (code=exited, status=0/SUCCESS)
        CPU: 5ms
...（略）...
```

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
625c625
< #track_counts = on
---
> track_counts = on
645c645
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
$ source ~/venv/rsl_base/bin/activate
(rsl_base) $ pip --version
pip 26.2.1 from /home/rsl/venv/rsl_base/lib/python3.12/site-packages/pip (python 3.12)

(rsl_base) $ pip freeze
...（略）...

(rsl_base) $ deactivate
$
```

## Visual Studio Code
```bash
$ code -v
1.134.0
110a328ea54b42367b803ec53ee0bf52ef26b419
x64
```

## Linux Mintの終了
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
      - **名前**: `Linux Mint 22 MATE 64-bit_initial`
      - **パス**: `X:\VirtualBox VMs`
   2. **すべてをクローン**を選択し、**次へ**ボタンをクリックする。
   3. **現在のマシンの状態**を選択し、**完了**ボタンをクリックする。
      - ...（3分程度）...
