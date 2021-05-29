# pgRoutingのインストール

## インストール
```bash
sudo apt install postgresql-12-pgrouting
```

## バージョンの確認
```bash
sudo -u postgres psql
```

```pgsql
\l
gistest
\d
CREATE EXTENSION pgrouting;
SELECT * FROM pgr_version();
\q
```
