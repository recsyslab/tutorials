# Docker

## 準備
```bash
$ sudo apt update
$ sudo apt install apt-transport-https
$ sudo apt install ca-certificates
$ sudo apt install curl
$ sudo apt install gnupg-agent
$ sudo apt install software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ cat /etc/os-release
# ...（略）...
UBUNTU_CODENAME=focal
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
# cat /etc/os-releaseで確認したUBUNTU_CODENAMEの値を設定する。
$ sudo apt-get update
```

## インストール
```bash
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## インストールの確認
```bash
$ sudo docker run hello-world
```

## ユーザのdockerグループへの追加
```bash
$ sudo gpasswd -a $USER docker
```

#### 参考
- [【簡単な4つの方法】UbuntuにDockerをインストールするには ｜Kinsta®](https://kinsta.com/jp/blog/install-docker-ubuntu/)
- [Linux Mint 20にDockerをインストールする – 山本隆の開発日誌](https://www.gesource.jp/weblog/?p=8493)
