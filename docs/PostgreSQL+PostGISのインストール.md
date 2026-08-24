# PostgreSQL+PostGISのインストール

## PostgreSQLとPostGISのインストール
```bash
$ sudo apt install postgresql
# ...（3分程度）...
$ sudo apt install postgis
# ...（3分程度）...
```

## PostgreSQLの動作確認とバージョンの確認
```bash
$ sudo -u postgres psql
psql (16.4 (Ubuntu 16.4-0ubuntu0.24.04.2))
Type "help" for help.

postgres=#
```

PostgreSQLにログインすると、プロンプトが、
```pgsql
postgres=# 
```
に変わる。`postgres`の部分は接続先データベース名を表す。以降、`【データベース】=#`と記載している箇所は、PostgreSQL上で`【データベース】`に接続した状態で入力するコマンドを表す。

```pgsql
postgres=# SELECT version();
                                                             version                                                             
---------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 16.4 (Ubuntu 16.4-0ubuntu0.24.04.2) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 13.2.0-23ubuntu4) 13.2.0, 64-bit
(1 row)

# インストールされたバージョンを確認する。
# 2024/09/10時点の最新版: 16.4
postgres=# \q
# '\'はキーボードの右下のバックスラッシュ「ろ」を押す（右上の'￥'ではない）
```

## 設定ファイルの準備

### /etc/postgresql-common/createcluster.confの準備
```bash
$ ls /etc/postgresql-common/
$ less /etc/postgresql-common/createcluster.conf
$ sudo cp /etc/postgresql-common/createcluster.conf /etc/postgresql-common/createcluster.conf-org
$ ls /etc/postgresql-common/
createcluster.conf      pg_upgradecluster.d  supported_versions
createcluster.conf-org  root.crt             user_clusters
```

※下記の`16`の箇所はPostgreSQLのバージョンを表す。インストールされたバージョンに合わせて適宜置換すること。例えば、バージョン14.xをインストールした場合は、`16`の箇所を`14`に置換する。以下、同様。

### /etc/postgresql/16/main/pg_hba.confの準備
```bash
$ ls /etc/postgresql/16/main/
$ sudo less /etc/postgresql/16/main/pg_hba.conf
$ sudo cp /etc/postgresql/16/main/pg_hba.conf /etc/postgresql/16/main/pg_hba.conf-org
$ ls /etc/postgresql/16/main/
conf.d       pg_ctl.conf  pg_hba.conf-org  postgresql.conf
environment  pg_hba.conf  pg_ident.conf    start.conf
```

### /etc/postgresql/16/main/postgresql.confの準備
```bash
$ ls /etc/postgresql/16/main/
$ less /etc/postgresql/16/main/postgresql.conf
$ sudo cp /etc/postgresql/16/main/postgresql.conf /etc/postgresql/16/main/postgresql.conf-org
$ ls /etc/postgresql/16/main/
conf.d       pg_ctl.conf  pg_hba.conf-org  postgresql.conf      start.conf
environment  pg_hba.conf  pg_ident.conf    postgresql.conf-org
```

## PostgreSQLサーバの起動設定
```bash
$ systemctl is-enabled postgresql
enabled
# 「enabled」と表示されれば、postgresqlの自動起動が有効になっている。
$ systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; prese>
     Active: active (exited) since Tue 2024-09-10 09:41:05 JST; 2min 20s ago
...（略）...
# 「Active: active (exited)」と表示されれば、postgresqlは稼働している。
```

## PostgreSQLのパスワードの設定
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# \password
Enter new password for user "postgres":
# パスワードを入力しても表示されないが、そのまま入力する。
Enter it again: 
# パスワードを入力しても表示されないが、そのまま入力する。
postgres=# \q
```

```bash
$ sudo less /etc/postgresql/16/main/pg_hba.conf
$ sudo vi /etc/postgresql/16/main/pg_hba.conf
```
`pg_hba.conf`の下記3箇所について`peer`を`md5`に書き換える。`vi`の代わりに`nano`で編集しても良い。

`/etc/postgresql/16/main/pg_hba.conf`
```txt
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
$ sudo less /etc/postgresql/16/main/pg_hba.conf
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
$ sudo systemctl reload postgresql
$ systemctl status postgresql
$ sudo -u postgres psql
# 設定したパスワードを入力してログインできることを確認する。
```

#### 参考
1. Qiita, [peer認証の関係でpsqlログインできない時の対処法](https://qiita.com/tomlla/items/9fa2feab1b9bd8749584)

## autovacuumの設定
```bash
$ sudo less /etc/postgresql/16/main/postgresql.conf
$ sudo vi /etc/postgresql/16/main/postgresql.conf
```

`/etc/postgresql/16/main/postgresql.conf`
```
...（略）...
track_counts = on
...（略）...
autovacuum = on
...（略）...
```

```bash
$ sudo less /etc/postgresql/16/main/postgresql.conf
$ sudo diff /etc/postgresql/16/main/postgresql.conf-org /etc/postgresql/16/main/postgresql.conf
620c620
< #track_counts = on
---
> track_counts = on
640c640
< #autovacuum = on			# Enable autovacuum subprocess?  'on'
---
> autovacuum = on			# Enable autovacuum subprocess?  'on'
$ sudo systemctl reload postgresql
$ systemctl status postgresql
```

#### 参考
1. RAKUS Developers Blog, [VACUUMでPostgreSQLのゴミデータをお掃除！](https://tech-blog.rakus.co.jp/entry/20221227/vacuum)

## rslユーザの作成
```pgsql
postgres=# CREATE ROLE rsl WITH LOGIN PASSWORD '【パスワード】';
```

## PostGISの動作テスト
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# \l
                                                       List of databases
   Name    |  Owner   | Encoding | Locale Provider |   Collate   |    Ctype    | ICU Locale | ICU Rules |   Access privileges   
-----------+----------+----------+-----------------+-------------+-------------+------------+-----------+-----------------------
 postgres  | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | 
 template0 | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | =c/postgres          +
           |          |          |                 |             |             |            |           | postgres=CTc/postgres
 template1 | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | =c/postgres          +
           |          |          |                 |             |             |            |           | postgres=CTc/postgres
(3 rows)

postgres=# CREATE DATABASE gistest ENCODING 'UTF8';
CREATE DATABASE
postgres=# \l
                                                       List of databases
   Name    |  Owner   | Encoding | Locale Provider |   Collate   |    Ctype    | ICU Locale | ICU Rules |   Access privileges   
-----------+----------+----------+-----------------+-------------+-------------+------------+-----------+-----------------------
 gistest   | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | 
 postgres  | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | 
 template0 | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | =c/postgres          +
           |          |          |                 |             |             |            |           | postgres=CTc/postgres
 template1 | postgres | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           | =c/postgres          +
           |          |          |                 |             |             |            |           | postgres=CTc/postgres
(4 rows)

postgres=# \c gistest
You are now connected to database "gistest" as user "postgres".
gistest=# \d
Did not find any relations.
gistest=# CREATE EXTENSION postgis;
CREATE EXTENSION
gistest=# SELECT postgis_full_version();
                                                                                                                                       postgis_full_version                                                                                                                                       
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 POSTGIS="3.4.2 c19ce56" [EXTENSION] PGSQL="160" GEOS="3.12.1-CAPI-1.18.1" PROJ="9.4.0 NETWORK_ENABLED=OFF URL_ENDPOINT=https://cdn.proj.org USER_WRITABLE_DIRECTORY=/tmp/proj DATABASE_PATH=/usr/share/proj/proj.db" LIBXML="2.9.14" LIBJSON="0.17" LIBPROTOBUF="1.4.1" WAGYU="0.5.0 (Internal)"
(1 row)

gistest=# CREATE TABLE sample(id INT, name TEXT, PRIMARY KEY(id));
CREATE TABLE
gistest=# CREATE TABLE gissample(id SERIAL, point GEOMETRY(POINT, 4326), line GEOMETRY(LINESTRING, 4326), polygon GEOMETRY(POLYGON, 4326));
CREATE TABLE
gistest=# INSERT INTO gissample(point, line, polygon) VALUES(ST_GeomFromText('POINT(50 50)', 4326), ST_GeomFromText('LINESTRING(1 1, 99 99)', 4326), ST_GeomFromText('POLYGON((25 25, 75 25, 75 75, 25 75, 25 25))', 4326));
INSERT 0 1
gistest=# SELECT ST_AsText(polygon) AS polygon FROM gissample;
                 polygon                  
------------------------------------------
 POLYGON((25 25,75 25,75 75,25 75,25 25))
(1 row)

gistest=# \d
                List of relations
 Schema |       Name        |   Type   |  Owner   
--------+-------------------+----------+----------
 public | geography_columns | view     | postgres
 public | geometry_columns  | view     | postgres
 public | gissample         | table    | postgres
 public | gissample_id_seq  | sequence | postgres
 public | sample            | table    | postgres
 public | spatial_ref_sys   | table    | postgres
(6 rows)

gistest=# \q
```

#### 参考
1. 黒い猫の開発日記, [PostGISを利用してみる。](https://cats-mew.hatenadiary.org/entry/20090811/1249976482)
   - ただし、`GeomFromText`は`ST_GeomFromText`に、`AsText`は`ST_AsText`に読み替える必要がある。
