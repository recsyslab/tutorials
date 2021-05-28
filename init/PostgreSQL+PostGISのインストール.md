# PostgreSQL+PostGISのインストール.md

## PostgreSQLリポジトリの追加
```bash
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main" > /etc/apt/sources.list.d/postgresql.list'
$ ls /etc/apt/sources.list.d/
$ less /etc/apt/sources.list.d/postgresql.list
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

## パッケージのアップデート
```bash
$ sudo apt clean
$ sudo apt update
$ sudo apt upgrade
# アップデートマネージャからのアップデートも実行しておく．
```

## PostgreSQL 11のインストール
1
$ sudo apt install postgresql-11
参考
Qiita, Ubuntu16にPostGISを導入するまでの流れ
Website for Students Learn Ubuntu Linux, Windows OS and CMS, How To Install PostgreSQL 11 On
r00t4bl3, HOW TO INSTALL POSTGRESQL 11 ON LINUX MINT 19 TARA
