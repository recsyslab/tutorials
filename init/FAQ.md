# FAQ

## Symantec Endpoint Protection (SEP) がインストールできない
「Downgrade is not supported. Please make sure the target version is newer than the original one.」というエラーが表示されたら、下記を実行した後、[セキュリティソフトのインストール](セキュリティソフトのインストール.md)にしたがって再度インストールを実行してみる。

1
2
3
4
$ sudo find / -name "*ymantec*"
$ sudo rm -rf /opt/Symantec/
$ sudo rm -rf /etc/symantec/
$ sudo rm -rf /var/symantec/
PostGIS 2.5のインストール（aptから）ができない
この方法ではうまくインストールできなかったので，[PostGIS 2.5のインストール（ソースから）]のインストールを実行する．

1
$ sudo apt install postgresql-11-postgis-2.5
PostGIS利用時にライブラリがロードできないというエラーが発生する
QGISのインストール後など，PostGIS利用時に下記のエラーが発生した場合，PostGISを再インストールすることで解決できることがある．

ERROR: ライブラリ"/usr/lib/postgresql/11/lib/postgis-2.5.so"をロードできませんでした: /usr/lib/postgresql/11/lib/postgis-2.5.so: undefined symbol: lwgeom_sfcgal_version
PostgreSQLを再インストールしたい
下記手順で既存のPostgreSQLをすべてアンインストールした後，[PostgreSQL+PostGISのインストール]にしたがってPostgreSQLを再インストールする．

1
2
3
4
5
$ ls /etc/postgresql/
$ sudo apt --purge remove postgresql-*
# 注）既存のPostgreSQLをアンインストールする．
$ sudo apt autoremove
$ ls /etc/postgresql/
Pythonを再インストールしたい
ライブラリ等を新たにインストールした場合は，下記手順によりPythonを再インストールする．

1
2
3
4
5
6
$ rm -rf ~/opt/python/
$ cd ~/src/Python-3.8.2/
$ ./configure --prefix=【HOME】/opt/python --with-ensurepip=install
# ...（3分程度）...
$ make 2>&1 | tee make.log
$ make altinstall
ウィンドウのタイトルバー（最小化，最大化，閉じるボタンがあるバー）が消えてしまった
[Menu] – [コントロールセンター]を開く．
[自動起動するアプリ]を開く．
[自動起動するプログラム]で[追加]ボタンをクリックし，下記を設定する．
[名前]]: Compiz
[コマンド]: compiz --replace
[追加]ボタンをクリックする．
[閉じる]ボタンをクリックする．
Linux Mintを再起動する．
参考
221B Baker Street, Linux Mint 18 MATE: MATE で「Compiz」を使う
Linuxとは日記, Ubuntuのウィンドウのボタンが消えた時の解決法
