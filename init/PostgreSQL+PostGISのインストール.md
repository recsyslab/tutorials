# PostgreSQL+PostGISのインストール.md

## PostgreSQL 12とPostGISのインストール
```bash
$ sudo apt install postgresql
$ sudo apt install postgis
  # ...（3分程度）...
```

## 設定ファイルの準備
`createcluster.conf`
```bash
$ ls /etc/postgresql-common/
$ less /etc/postgresql-common/createcluster.conf
$ sudo cp /etc/postgresql-common/createcluster.conf /etc/postgresql-common/createcluster.conf-org
$ ls /etc/postgresql-common/
```

`pg_hba.conf`
```bash
$ ls /etc/postgresql/12/main/
$ sudo less /etc/postgresql/12/main/pg_hba.conf
$ sudo cp /etc/postgresql/12/main/pg_hba.conf /etc/postgresql/12/main/pg_hba.conf-org
$ ls /etc/postgresql/12/main/
```

## PostgreSQLの動作確認とバージョンの確認
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# SELECT version();
# インストールしたバージョンが表示されればOK。
postgres=# \q
# '\'はキーボードの右下のバックスラッシュ「ろ」を押す（右上の'￥'ではない）
```

## PostgreSQLサーバの起動設定
```bash
$ sudo sysv-rc-conf
$ sudo sysv-rc-conf postgresql off
$ sudo sysv-rc-conf
$ less ~/bin/startup.sh
$ echo -e '\n# PostgreSQLサーバの起動' >> ~/bin/startup.sh
$ echo -e 'sudo service postgresql start\n' >> ~/bin/startup.sh
$ less ~/bin/startup.sh
$ ~/bin/startup.sh
$ service postgresql status
```

## PostgreSQLのパスワードの設定
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# \password
postgres=# \q
```

```bash
$ sudo less /etc/postgresql/12/main/pg_hba.conf
$ sudo vi /etc/postgresql/12/main/pg_hba.conf
```
1. `pg_hba.conf`の下記3箇所について`peer`を`md5`に書き換える。

`pg_hba.conf`
```bash
...（略）...
# Database administrative login by Unix domain socket
local all postgres md5
...（略）...
# "local" is for Unix domain socket connections only
local all all md5
...（略）...
# Allow replication connections from localhost, by a user with the
# replication privilege.
local replication all md5
...（略）...
```

```bash
$ sudo less /etc/postgresql/12/main/pg_hba.conf
$ sudo diff /etc/postgresql/12/main/pg_hba.conf-org /etc/postgresql/12/main/pg_hba.conf
$ sudo service postgresql restart
$ sudo -u postgres psql
```

```pgsql
postgres=# \q
# 設定したパスワードを入力してログインできることを確認する。
```

#### 参考
- Qiita, [peer認証の関係でpsqlログインできない時の対処法](https://qiita.com/tomlla/items/9fa2feab1b9bd8749584)

## PostGISの動作テスト
```pgsql
SELECT * FROM gistest;
postgres=# \l
postgres=# CREATE DATABASE gistest ENCODING 'UTF8';
postgres=# \l
postgres=# \c gistest
gistest=# \d
CREATE EXTENSION postgis;
gistest=# SELECT postgis_full_version();
gistest=# CREATE TABLE sample(id INT, name TEXT, PRIMARY KEY(id));
gistest=# CREATE TABLE gissample(id SERIAL, point GEOMETRY(POINT, 4326), line GEOMETRY(LINESTRING, 4326), polygon GEOMETRY(POLYGON, 4326));
gistest=# INSERT INTO gissample(point, line, polygon) VALUES(ST_GeomFromText('POINT(50 50)', 4326), ST_GeomFromText('LINESTRING(1 1, 99 99)', 4326), ST_GeomFromText('POLYGON((25 25, 75 25, 75 75, 25 75, 25 25))', 4326));
gistest=# SELECT ST_AsText(polygon) AS polygon FROM gissample;
gistest=# \d
gistest=# \q
```

#### 参考
- 黒い猫の開発日記, [PostGISを利用してみる。](https://cats-mew.hatenadiary.org/entry/20090811/1249976482)
  - ただし、`GeomFromText`は`ST_GeomFromText`に、`AsText`は`ST_AsText`に読み替える必要がある。
