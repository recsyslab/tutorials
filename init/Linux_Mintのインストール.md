# Linux Mintのインストール

## Linux Mint 21.2のインストール
1. タスクトレイの**ネットワークアイコン**を右クリックし、**Edit Connections**を開く。
   1. 使用する`Wired connection ＊`を選択する。
   2. 下の**Edit the selected connection**ボタンをクリックする。
      1. **IPv4 Settings**タブを開く。
         1. 下記を設定する。
            - DHCPの場合: # 通常はこちら
              - **Method**: `Automatic (DHCP)`
            - 固定IPアドレスの場合:
              - **Method**: `Manual`
              - **Address**: 【指定のIPアドレス】
              - **Netmask**: 【指定のサブネットマスク】
              - **Gateway**: 【指定のゲートウェイ】
              - **DNS Servers**: 【指定のDNSサーバ】
      2. **Save**ボタンをクリックする。
   3. 右上の**Close Window**ボタンをクリックする。
2. Linux Mintデスクトップから`Install Linux Mint`を実行する。
   1. **Install**ダイアログにしたがって下記を設定する。
      - `日本語`
      - **キーボードレイアウト**: `Japanese > Japanese - Japanese(OADG 109A)`
      - **マルチメディアコーデックのインストール**: `非チェック`
      - **ディスクを削除してLinux Mintをインストール**: ``選択``
   2. **インストール**ボタンをクリックする。
      - 「ディスクに変更を書き込みますか？」と聞かれたら**続ける**ボタンをクリックする。
   3. 下記を設定する。
      - **どこに住んでいますか？**: `Tokyo`
      - **あなたの名前**: `rsl`
      - **コンピューターの名前**: `recsyslab-mint`
      - **ユーザー名の入力**: `rsl`
      - **パスワードの入力**: `ryukoku` # ここではパスワードをサンプルとして`ryukoku`としているが、任意のパスワードを設定すること。
      - **ログイン時にパスワードを要求する**: `選択`
   4. **続ける**ボタンをクリックする。
      - （インストールが開始されるので、終了するまで待つ。30分程度。）
   5. インストールが完了したら、**今すぐ再起動する**ボタンをクリックする。
   6. 「Please remove the installation medium, then press ENTER:」と表示されるので、**ENTER**キーを押す。
      - USBメモリからインストールを実行している場合は、USBメモリを取り外したうえで**ENTER**キーを押す。
3. Linuxを終了する。
