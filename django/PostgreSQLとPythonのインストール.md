# PostgreSQLとPythonのインストール

## パッケージのアップグレード
```bash
rsl@＊:~$ sudo apt update
rsl@＊:~$ sudo apt upgrade
```

## PostgreSQL+PostGISのインストール

### PostgreSQLとPostGISのインストール
```bash
rsl@＊:~$ sudo apt install postgresql
rsl@＊:~$ sudo apt install postgis
```

### 設定ファイルの準備
※下記の`14`の箇所はPostgreSQLのバージョンを表す。インストールされたバージョンに合わせて適宜置換すること。例えば、バージョン12.xをインストールした場合は、`14`の箇所を`12`に置換する。以下、同様。

```bash
$ sudo cp /etc/postgresql-common/createcluster.conf /etc/postgresql-common/createcluster.conf-org
$ sudo cp /etc/postgresql/14/main/pg_hba.conf /etc/postgresql/14/main/pg_hba.conf-org
$ sudo cp /etc/postgresql/14/main/postgresql.conf /etc/postgresql/14/main/postgresql.conf-org
```

### PostgreSQLサーバの起動設定
```bash
rsl@＊:~$ systemctl is-enabled postgresql
# 「enabled」と表示されれば、postgresqlの自動起動が有効になっている。
rsl@＊:~$ systemctl status postgresql
# 「Active: active (exited)」と表示されれば、postgresqlは稼働している。
```

### PostgreSQLのパスワードの設定
```bash
rsl@＊:~$ sudo -u postgres psql
```

```pgsql
postgres=# \password
Enter new password for user "postgres":
Enter it again: 
postgres=# \q
```

```bash
rsl@＊:~$ sudo vi /etc/postgresql/14/main/pg_hba.conf
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
rsl@＊:~$ sudo diff /etc/postgresql/14/main/pg_hba.conf-org /etc/postgresql/14/main/pg_hba.conf
rsl@＊:~$ sudo systemctl reload postgresql
rsl@＊:~$ systemctl status postgresql
rsl@＊:~$ sudo -u postgres psql
# 設定したパスワードを入力してログインできることを確認する。
```

### autovacuumの設定

```bash
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
rsl@＊:~$ sudo diff /etc/postgresql/14/main/postgresql.conf-org /etc/postgresql/14/main/postgresql.conf
rsl@＊:~$ sudo systemctl reload postgresql
rsl@＊:~$ systemctl status postgresql
```
