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

## MySQLのバージョンの確認
```bash
mysql --version
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
sudo mysqladmin -u root password
```

```bash
sudo mysql -u root -p
# 設定したパスワードでログインできることを確認する。
```

## MySQLのユーザの作成
```mysql
CREATE USER 'rsl'@'localhost' IDENTIFIED BY '【パスワード】';
GRANT ALL PRIVILEGES ON *.* TO 'rsl'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

#### 参考
- 最新IT技術情報_arkgame.com, [Ubuntu 22.04 LTSにMySQLをインストールする方法](https://arkgame.com/2022/05/04/post-307738/)

