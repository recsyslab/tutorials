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

### `/etc/postgresql-common/createcluster.conf`の準備
```bash
$ ls /etc/postgresql-common/
$ less /etc/postgresql-common/createcluster.conf
$ sudo cp /etc/postgresql-common/createcluster.conf /etc/postgresql-common/createcluster.conf-org
$ ls /etc/postgresql-common/
createcluster.conf      pg_upgradecluster.d  supported_versions
createcluster.conf-org  root.crt             user_clusters
```

※下記の`16`の箇所はPostgreSQLのバージョンを表す。インストールされたバージョンに合わせて適宜置換すること。例えば、バージョン14.xをインストールした場合は、`16`の箇所を`14`に置換する。以下、同様。

### `/etc/postgresql/16/main/pg_hba.conf`の準備
```bash
$ ls /etc/postgresql/16/main/
$ sudo less /etc/postgresql/16/main/pg_hba.conf
$ sudo cp /etc/postgresql/16/main/pg_hba.conf /etc/postgresql/16/main/pg_hba.conf-org
$ ls /etc/postgresql/16/main/
conf.d       pg_ctl.conf  pg_hba.conf-org  postgresql.conf
environment  pg_hba.conf  pg_ident.conf    start.conf
```

### `/etc/postgresql/16/main/postgresql.conf`の準備
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
# 「enabled」と表示されれば、postgresqlの自動起動が有効になっている。
$ systemctl status postgresql
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
$ sudo less /etc/postgresql/14/main/pg_hba.conf
$ sudo vi /etc/postgresql/14/main/pg_hba.conf
```
`pg_hba.conf`の下記3箇所について`peer`を`md5`に書き換える。`vi`の代わりに`nano`で編集しても良い。

`/etc/postgresql/14/main/pg_hba.conf`
```
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
$ sudo less /etc/postgresql/14/main/pg_hba.conf
$ sudo diff /etc/postgresql/14/main/pg_hba.conf-org /etc/postgresql/14/main/pg_hba.conf
$ sudo systemctl reload postgresql
$ systemctl status postgresql
$ sudo -u postgres psql
# 設定したパスワードを入力してログインできることを確認する。
```

#### 参考
- Qiita, [peer認証の関係でpsqlログインできない時の対処法](https://qiita.com/tomlla/items/9fa2feab1b9bd8749584)

## autovacuumの設定
```bash
$ sudo less /etc/postgresql/14/main/postgresql.conf
$ sudo vi /etc/postgresql/14/main/postgresql.conf
```

`/etc/postgresql/14/main/postgresql.conf`
```
...（略）...
track_counts = on
...（略）...
autovacuum = on
...（略）...
```

```bash
$ sudo less /etc/postgresql/14/main/postgresql.conf
$ sudo diff /etc/postgresql/14/main/postgresql.conf-org /etc/postgresql/14/main/postgresql.conf
$ sudo systemctl reload postgresql
$ systemctl status postgresql
```

#### 参考
- RAKUS Developers Blog, [VACUUMでPostgreSQLのゴミデータをお掃除！](https://tech-blog.rakus.co.jp/entry/20221227/vacuum)

## PostGISの動作テスト
```bash
$ sudo -u postgres psql
```

```pgsql
postgres=# \l
postgres=# CREATE DATABASE gistest ENCODING 'UTF8';
postgres=# \l
postgres=# \c gistest
gistest=# \d
gistest=# CREATE EXTENSION postgis;
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
