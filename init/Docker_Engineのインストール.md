# Docker Engineのインストール

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

## Docker Engineの動作確認

### hello-world環境の実行
```bash
$ sudo docker run hello-world
# 「Hello from Docker!」と表示されれば成功
```

### 稼働状況の確認
```bash
$ sudo docker info
```

### Dockerイメージのダウンロード
```bash
$ sudo docker search --filter "stars=3" --filter "is-official=true" fedora
$ sudo docker pull fedora
```

### Dockerコンテナの起動
```bash
$ sudo docker run --rm -it fedora bash
[root@7622fb31f324 /]# cat /etc/redhat-release 
Fedora release 39 (Thirty Nine)
[root@7622fb31f324 /]# exit
```

### Dockerコンテナの一覧表示／停止／再起動／削除

#### コンテナの一覧表示
```bash
$ sudo docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
2bbc5ea45c20   hello-world   "/hello"   14 minutes ago   Exited (0) 14 minutes ago             infallible_perlman
```

#### コンテナの起動
```bash
$ sudo docker start 2bbc5ea45c20
```

#### コンテナの停止
```bash
$ sudo docker stop 2bbc5ea45c20
```

#### コンテナの再起動
```bash
$ sudo docker restart 2bbc5ea45c20
```

#### コンテナの削除
```bash
$ sudo docker rm 2bbc5ea45c20
```

### Dockerイメージの表示／削除

#### イメージの表示
```bash
$ sudo docker images
```

#### イメージの削除
```bash
$ sudo docker rmi fedora
```

## ユーザのdockerグループへの追加
```bash
$ groups rsl
$ sudo usermod -a -G docker rsl
$ groups rsl
$ sudo reboot
```

#### 参考
- 鶴長鎮一，『作りながら学ぶ Webシステムの教科書』，日経BP，2023．

## VSCodeとコンテナの連携

### Dev Containersのインストール
1. VSCodeの左メニューから**Extensions**を開く。
   1. 「containers」で検索する。
   2. **Dev Containers**を選択する。
   3. **Install**ボタンをクリックする。

### yarnのインストール
1. VSCodeの左メニューから**Extensions**を開く。
   1. 「yarn」で検索する。
   2. **yarn**を選択する。
   3. **Install**ボタンをクリックする。

#### 参考
- 株式会社オープントーン，佐藤大輔，伊東直喜，上野啓二，『実装で学ぶフルスタックWeb開発 エンジニアの視野と知識を広げる「一気通貫」型ハンズオン』，翔泳社，2023．
