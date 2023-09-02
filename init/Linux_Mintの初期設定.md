# Linux Mintの初期設定

## Linuxへのログイン
1. Oracle VM VirtualBox マネージャーを起動する。
   1. 対象の仮想マシンを選択し、起動ボタンをクリックする。
      1. Linuxにログインする。

## 端末の実行
1. Linuxデスクトップ左下の**端末**アイコンをクリックする。

端末を開くと、
```bash
rsl@recsyslab-mint:~$ 
```
と表示される。`$`までの部分はプロンプトといい、コマンド入力待ちを表す。本チュートリアルでは、
```bash
$ 【コマンド】
```
のように、`$`より前の部分は省略している。`【コマンド】`部分の内容を端末に入力し実行する。一つ一つのコマンドの意味を理解しながら打ち込んでいくと良い。

## ディレクトリの準備
- `~/bin/`: 独自のシェルスクリプトを置いておく。
- `~/src/`: ソースからインストールする際に必要なファイルを置いておく。
- `~/opt/`: 追加したアプリケーションを置いておく。
```bash
$ ls
$ mkdir ~/bin/
$ mkdir ~/src/
$ mkdir ~/opt/
$ ls
```

## 設定ファイルの準備
設定ファイルとして、`.profile`と`.bashrc`の準備をしておく。それぞれの初期状態を`.profile-org`, `.bashrc-org`としてバックアップしておく。今後、設定ファイルを更新する場合は、`.profile`, `.bashrc`の`Add below.`以下に追記していく。
- `.profile`: ログイン時に1度だけ読み込まれる。
- `.bashrc`: bash起動の度に読み込まれる。

### `.profile`の準備
```bash
$ ls -a
$ less ~/.profile
$ cp ~/.profile ~/.profile-org
$ ls -a
$ echo -e '\n\n#### #### Add below. #### ####' >> ~/.profile
$ less ~/.profile
$ less ~/.profile-org
$ diff ~/.profile-org ~/.profile
```

### `.bashrc`の準備
```bash
$ ls -a
$ less ~/.bashrc
$ cp ~/.bashrc ~/.bashrc-org
$ ls -a
$ echo -e '\n\n#### #### Add below. #### ####' >> ~/.bashrc
$ less ~/.bashrc
$ less ~/.bashrc-org
$ diff ~/.bashrc-org ~/.bashrc
```

#### 参考
- OXY NOTES, [ユーザーの環境変数を設定するbashの設定ファイルと、カスタムプロンプトについて](https://oxynotes.com/?p=5418)

### `/etc/apt/sources.list`のバックアップ
```bash
$ ls /etc/apt/
$ less /etc/apt/sources.list
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list-org
$ ls /etc/apt/
$ sudo sh -c 'echo "\n\n#### #### Add below. #### ####" >> /etc/apt/sources.list'
$ less /etc/apt/sources.list
$ less /etc/apt/sources.list-org
$ diff /etc/apt/sources.list-org /etc/apt/sources.list
```

#### 参考
- Linuxゲリラ戦記, [/etc/apt/sources.list（パッケージのダウンロード元設定ファイル・Debian）](https://www.garunimo.com/program/linux/_etc_apt_sources_list.php)

## 正確な時刻を設定する
時刻が正確でないと、何らかのプログラムを実行する際に不具合が生じることがある。
```bash
$ date
$ sudo ntpdate ntp.nict.jp
$ date
```

#### 参考
- ＠IT, [【 ntpdate 】コマンド――時刻をNTPサーバと同期する](https://www.atmarkit.co.jp/ait/articles/1906/21/news013.html)

## パッケージのアップグレード
`apt upgrade`によるアップグレードとアップデートマネージャによるアップグレードがある。両方を定期的に実行しておく。

### `apt upgrade`によるアップグレード
```bash
sudo apt update
sudo apt upgrade
# ...（20分程度）...
```

### アップデートマネージャによるアップグレード
1. タスクトレイの**アップデートマネージャ**を起動する。
   1. **更新**ボタンをクリックする。
   2. **アップデートをインストール**ボタンをクリックする。
      - ...（10分程度）...
      - 「このシステムは最新の状態です」と表示されればOK。
      - 「大量のエラーが発生したため、処理が停止しました。」というエラーが出た場合、アップデートマネージャでのアップデートを実行した後、再度試すとうまくいくことがある。
      - アップデートでエラーが発生することもあるが、何度か試すと成功することが多い。

## ディレクトリ名を英語表記に変更
```bash
ls
LANG=C xdg-user-dirs-gtk-update
# ダイアログが表示されるので、**Update Names**ボタンをクリックする。
ls
# ディレクトリ名が英語になっていることを確認する．
```

#### 参考
- Qiita, [「デスクトップ」等のディレクトリ名を英語にする](https://qiita.com/take5249/items/13ada73bbd01ee12a2c3)

## `sysv-rc-conf`のインストール
```bash
cd ~/src/
wget http://archive.ubuntu.com/ubuntu/pool/universe/s/sysv-rc-conf/sysv-rc-conf_0.99.orig.tar.gz
tar zxvf sysv-rc-conf_0.99.orig.tar.gz
cd sysv-rc-conf-0.99
sudo make
sudo make install
sudo apt install libcurses-ui-perl libterm-readkey-perl libcurses-perl
sudo sysv-rc-conf
# [h]キーでバージョンを確認できる．
cd ../
rm -f sysv-rc-conf_0.99.orig.tar.gz
```

#### 参考
- Qiita, [Ubuntu18.04 LTSにsysv-rc-confを入れる方法](https://gcga.jp/blog/2018/08/10/205/)

## ファイアウォールの設定
```bash
sudo apt install gufw
sudo ufw status
sudo ufw enable
sudo ufw default deny
sudo ufw status
```

## スタートアップシェルスクリプトの作成
サーバの起動やドライブのマウントなど、Linux起動時に行いたいことはスタートアップシェルスクリプトに記述しておくと良い。下記では、スタートアップシェルスクリプトとして`startup.sh`を作成しておく。今後は、必要に応じて、このファイルに追記していく。
```bash
echo -e '#! /bin/sh\n' >> ~/bin/startup.sh
ls -l ~/bin/
chmod +x ~/bin/startup.sh
ls -l ~/bin/
less ~/bin/startup.sh
```
Linux起動時に、下記コマンドを実行すると良い。
```bash
~/bin/startup.sh
```

## Windows共有フォルダのマウント # VirtualBox経由の場合
### Cドライブ
```bash
ls /mnt/
less ~/bin/startup.sh
sudo mkdir -p /mnt/c/
echo -e '\n# Cドライブのマウント' >> ~/bin/startup.sh
echo -e 'sudo umount C_DRIVE' >> ~/bin/startup.sh
echo -e 'sudo mount -t vboxsf C_DRIVE /mnt/c/' >> ~/bin/startup.sh
echo -e 'df -h\n' >> ~/bin/startup.sh
less ~/bin/startup.sh
~/bin/startup.sh
ls /mnt/c/
```

### Dドライブ（外付けSSD）
```bash
ls /mnt/
less ~/bin/startup.sh
sudo mkdir -p /mnt/d/
echo -e '\n# Dドライブのマウント' >> ~/bin/startup.sh
echo -e 'sudo umount D_DRIVE' >> ~/bin/startup.sh
echo -e 'sudo mount -t vboxsf D_DRIVE /mnt/d/' >> ~/bin/startup.sh
echo -e 'df -h\n' >> ~/bin/startup.sh
less ~/bin/startup.sh
~/bin/startup.sh
ls /mnt/d/
```

## gitのインストール
```bash
sudo apt install git
```
