# OpenStreetMapデータのインポート

## データベースの作成 # 関西圏の場合
```pgsql
CREATE DATABASE osm_kansai ENCODING 'UTF8';
\c osm_kansai
CREATE EXTENSION postgis;
```

## `osm2pgsql`のインストール
```bash
sudo apt install osm2pgsql
```

## pbfファイルのダウンロード
1. 下記から対象地域のosmデータ（pbfファイル）をダウンロードする。
   - [Download OpenStreetMap data for this region (Kansai region (a.k.a. Kinki region)) ](http://download.geofabrik.de/asia/japan/kansai.html)
   - `kansai-latest.osm.pbf`

## pbfファイルの読込み
```bash
cd ~/Downloads
sudo vi /etc/postgresql/11/main/pg_hba.conf
```

一時的に`pg_hba.conf`の下記4箇所について`md5`を`trust`に書き換える。
`pg_hba.conf`
```
...（略）...
#local   all             postgres                                md5
local   all             postgres                                trust
...（略）...
#local   all             all                                     md5
local   all             all                                     trust
...（略）...
#host    all             all             127.0.0.1/32            md5
host    all             all             127.0.0.1/32            trust
...（略）...
#host    all             all             ::1/128                 md5
host    all             all             ::1/128                 trust
...（略）...
```

```bash
sudo service postgresql restart
osm2pgsql -c -d osm_kansai -U postgres --slim -C 4200 --cache-strategy sparse --host localhost kansai-latest.osm.pbf
# ...（kansaiの場合，15分程度）...
# ...（japanの場合，1時間20分程度）...
sudo vi /etc/postgresql/11/main/pg_hba.conf
```

一時的に書き換えていた`pg_hba.conf`の下記4箇所について`trust`を`md5`に戻す。
`pg_hba.conf`
```
...（略）...
local   all             postgres                                md5
#local   all             postgres                                trust
...（略）...
local   all             all                                     md5
#local   all             all                                     trust
...（略）...
host    all             all             127.0.0.1/32            md5
#host    all             all             127.0.0.1/32            trust
...（略）...
host    all             all             ::1/128                 md5
#host    all             all             ::1/128                 trust
...（略）...
```

```bash
sudo service postgresql restart
```

## 取り込まれたテーブルの確認
下記のようにリレーション一覧が表示されればOK。
```pgsql
\c osm_kansai
\dt
                  リレーション一覧
 スキーマ |        名前        |    型    |  所有者  
----------+--------------------+----------+----------
 public   | planet_osm_line    | テーブル | postgres
 public   | planet_osm_nodes   | テーブル | postgres
 public   | planet_osm_point   | テーブル | postgres
 public   | planet_osm_polygon | テーブル | postgres
 public   | planet_osm_rels    | テーブル | postgres
 public   | planet_osm_roads   | テーブル | postgres
 public   | planet_osm_ways    | テーブル | postgres
 public   | spatial_ref_sys    | テーブル | postgres
(8 行)
```

#### 参考
- GEOFABRIK downloads, [OpenStreetMap: Download OpenStreetMap data for this region: Japan](http://download.geofabrik.de/asia/japan.html)
- JURI GIS, [OSMのデータをPostGISのDBに入れてみる](http://www.jurigis.me/2015/01/12/osm-download/)
- OpenStreetMap Wiki, [JA:Map Features](https://wiki.openstreetmap.org/wiki/JA:Map_Features)
