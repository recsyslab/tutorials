# MySQLのインストール

## システムパッケージの更新
```bash
sudo apt update
sudo apt upgrade
```

## MySQL 8のインストール
```bash
sudo apt install mysql-server
```

## MySQLの動作確認とバージョンの確認
```bash
sudo mysql
```

```mysql
SELECT version();
# インストールしたバージョンが表示されればOK。
exit
```

## MySQLサーバの起動設定
```bash
sudo sysv-rc-conf
sudo sysv-rc-conf mysql off
sudo sysv-rc-conf
less ~/bin/startup.sh
echo -e '\n# MySQLサーバの起動' >> ~/bin/startup.sh
echo -e 'sudo service mysql start\n' >> ~/bin/startup.sh
less ~/bin/startup.sh
~/bin/startup.sh
service mysql status
```

## MySQLのパスワードの設定
```bash
sudo mysql
```

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '【パスワード】';
FLUSH PRIVILEGES;
exit
```

```bash
mysql -u root -p
# 設定したパスワードでログインできることを確認する。
```

## MySQLのユーザの作成
```mysql
CREATE USER 'rsl'@'localhost' IDENTIFIED BY '【パスワード】';
GRANT ALL PRIVILEGES ON *.* TO 'rsl'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
