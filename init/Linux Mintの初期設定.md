# Linux Mintの初期設定

## ディレクトリの準備

- `~/bin/`: 独自のシェルスクリプトを置いておく。
- `~/src/`: ソースからインスト―する際に必要なファイルを置いておく。
- `~/opt/`: 追加したアプリケーションを置いておく。

```bash
$ mkdir ~/bin/
$ mkdir ~/src/
$ mkdir ~/opt/
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

### 参考
- OXY NOTES, [ユーザーの環境変数を設定するbashの設定ファイルと、カスタムプロンプトについて](https://oxynotes.com/?p=5418)
