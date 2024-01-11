# Docker

## インストール

### 準備
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

### Dockerのインストール
```bash
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### インストールの確認
```bash
$ sudo docker run hello-world
```

### ユーザのdockerグループへの追加
```bash
$ sudo gpasswd -a $USER docker
```

#### 参考
- [【簡単な4つの方法】UbuntuにDockerをインストールするには ｜Kinsta®](https://kinsta.com/jp/blog/install-docker-ubuntu/)
- [Linux Mint 20にDockerをインストールする – 山本隆の開発日誌](https://www.gesource.jp/weblog/?p=8493)

## 基本操作
### イメージの確認
```bash
$ sudo docker images
```

### イメージの取得
```bash
$ sudo docker pull ubuntu
$ sudo docker images
```

### コンテナの作成
```bash
$ sudo docker ps
$ sudo docker run -it ubuntu
root@03a42e59d160:/# 
[Ctrl+P, Q]
$ sudo docker ps
$ sudo docker attach 03a42e59d160
root@03a42e59d160:/# exit
$ sudo docker ps
```

### コンテナの起動
```bash
$ sudo docker start 03a42e59d160
03a42e59d160
$ sudo docker ps -a
```

### コンテナの停止
```bash
$ sudo docker stop 03a42e59d160
03a42e59d160
$ sudo docker ps -a
```

#### 参考
- [Dockerコンテナの作成、起動〜停止まで #Docker - Qiita](https://qiita.com/kooohei/items/0e788a2ce8c30f9dba53)
