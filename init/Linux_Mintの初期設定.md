# Linux Mintの初期設定

## Linuxへのログイン
1. **Oracle VM VirtualBox マネージャー**を起動する。
   1. 対象の仮想マシンを選択し、**起動**ボタンをクリックする。
      1. Linuxにログインする。

## 端末の実行
1. Linux Mintデスクトップ左下の**端末**アイコンをクリックする。

端末を開くと、
```bash
rsl@recsyslab-mint:~$ 
```
と表示される。`$`までの部分はプロンプトとよび、コマンド入力待ちを表す。本チュートリアルでは、
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
bin  src           テンプレート  ドキュメント  ピクチャ      公開
opt  ダウンロード  デスクトップ  ビデオ        ミュージック
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
$ sudo apt install ntpdate
$ sudo ntpdate ntp.nict.jp
$ date
```

#### 参考
- ＠IT, [【 ntpdate 】コマンド――時刻をNTPサーバと同期する](https://www.atmarkit.co.jp/ait/articles/1906/21/news013.html)

## パッケージのアップグレード
`apt upgrade`によるアップグレードとアップデートマネージャによるアップグレードがある。両方を定期的に実行しておく。

### `apt upgrade`によるアップグレード
```bash
$ sudo apt update
$ sudo apt upgrade
# ...（20分程度）...
# 「大量のエラーが発生したため、処理が停止しました。」というエラーが出た場合、下記の**アップデートマネージャーでのアップデート**を実行した後、再度試すとうまくいくことがある。
# アップデートでエラーが発生することもあるが、何度か試すと成功することが多い。
```

### アップデートマネージャーによるアップグレード
1. Linux Mintデスクトップ右下のタスクトレイの**アップデートマネージャー**を起動する。
   1. **アップデートマネージャーへようこそ**画面が表示されたら、**OK**ボタンをクリックする。
   2. 「システムは最新です」と表示されればOK。
      1. 最新でなければ、**アップデートをインストール**ボタンをクリックする。
         - ...（10分程度）...

## ディレクトリ名を英語表記に変更
```bash
$ ls
$ LANG=C xdg-user-dirs-gtk-update
# ダイアログが表示されるので、**Update Names**ボタンをクリックする。
$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos  bin  opt  src
# ディレクトリ名が英語になっていることを確認する．
```

#### 参考
- Qiita, [「デスクトップ」等のディレクトリ名を英語にする](https://qiita.com/take5249/items/13ada73bbd01ee12a2c3)

## ファイアウォールの設定
```bash
$ sudo apt install gufw
$ sudo ufw status
状態: 非アクティブ
$ sudo ufw enable
ファイアウォールはアクティブかつシステムの起動時に有効化されます。
$ sudo ufw default deny
デフォルトの incoming ポリシーは 'deny' に変更しました
(適用したい内容に基づいて必ずルールを更新してください)
$ sudo ufw status
状態: アクティブ
```

## スタートアップシェルスクリプトの作成
サーバの起動やドライブのマウントなど、Linux起動時に行いたいことはスタートアップシェルスクリプトに記述しておくと良い。下記では、スタートアップシェルスクリプトとして`startup.sh`を作成しておく。今後は、必要に応じて、このファイルに追記していく。
```bash
$ echo -e '#! /bin/sh\n' >> ~/bin/startup.sh
$ ls -l ~/bin/
合計 4
-rw-rw-r-- 1 rsl rsl 12  9月  9 18:47 startup.sh
$ chmod +x ~/bin/startup.sh
$ ls -l ~/bin/
合計 4
-rwxrwxr-x 1 rsl rsl 12  9月  9 18:47 startup.sh
$ less ~/bin/startup.sh
```

Linux起動時に下記コマンドを実行することで、必要な初期設定を一括で実行することができる。
```bash
$ ~/bin/startup.sh
```

## Windows共有フォルダのマウント
[VirtualBoxの環境設定](VirtualBoxの環境設定.md)であらかじめ共有フォルダーを追加しておくこと。

### Cドライブのマウント
```bash
$ ls /mnt/
$ less ~/bin/startup.sh
$ sudo mkdir -p /mnt/c/
$ echo -e '\n# Cドライブのマウント' >> ~/bin/startup.sh
$ echo -e 'sudo umount C_DRIVE' >> ~/bin/startup.sh
$ echo -e 'sudo mount -t vboxsf C_DRIVE /mnt/c/' >> ~/bin/startup.sh
$ echo -e 'df -h\n' >> ~/bin/startup.sh
$ less ~/bin/startup.sh
$ ~/bin/startup.sh
Filesystem      Size  Used Avail Use% Mounted on
...（略）...
C_DRIVE         476G  456G   21G  96% /mnt/c
$ ls /mnt/c/
$ ls /mnt/x/
```

### Xドライブ（外付けSSD）のマウント
```bash
$ ls /mnt/
$ less ~/bin/startup.sh
$ sudo mkdir -p /mnt/x/
$ echo -e '\n# Xドライブのマウント' >> ~/bin/startup.sh
$ echo -e 'sudo umount X_DRIVE' >> ~/bin/startup.sh
$ echo -e 'sudo mount -t vboxsf X_DRIVE /mnt/x/' >> ~/bin/startup.sh
$ echo -e 'df -h\n' >> ~/bin/startup.sh
$ less ~/bin/startup.sh
$ ~/bin/startup.sh
Filesystem      Size  Used Avail Use% Mounted on
...（略）...
C_DRIVE         476G  456G   21G  96% /mnt/c
X_DRIVE         895G  878G   18G  99% /mnt/x
```
