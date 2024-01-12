# Dockerのインストール

## 事前準備

### リポジトリの追加に必要なパッケージのインストール
```bash
$ sudo apt update
$ sudo apt install ca-certificates curl gnupg lsb-release
```

### Dockerの公式GPG鍵の追加
```bash
$ ls /etc/apt/
$ sudo mkdir -p /etc/apt/keyrings/
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ ls /etc/apt/keyrings/
```

### Docker（安定版）のリポジトリの追加
```bash
$ cat /etc/os-release
# ...（略）...
UBUNTU_CODENAME=jammy
$ ls /etc/apt/sources.list.d/
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu jammy stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# `jammy`の箇所には、`cat /etc/os-release`で確認した`UBUNTU_CODENAME`の値を指定する。
$ ls /etc/apt/sources.list.d/
```

### パッケージの更新
```bash
$ sudo apt update
```

## Docker Engine最新版のインストール
```bash
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

#### 参考
- 鶴長鎮一，『作りながら学ぶ Webシステムの教科書』，日経BP，2023．
