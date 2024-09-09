# VirtualBoxのインストール

## VirtualBoxのインストール

1. 下記からVirtualBoxをダウンロードする。
   - **[Downloads – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)**
   - **VirtualBox 7.＊.＊＊ platform packages > Windows hosts**: `VirtualBox-7.＊.＊＊-＊-Win.exe`
2. `VirtualBox-7.＊.＊＊-＊-Win.exe`を実行する。
   1. ダイアログにしたがって任意に設定し、**Install**ボタンをクリックする。
   2. インストールが完了したら、**Finish**ボタンをクリックする。

## VirtualBox上でのLinux Mint 22 MATE 64-bitの仮想マシンの構築
1. 下記からLinux Mint 22のISOファイルをダウンロードし、`X:\Downloads\`ディレクトリに置く。
   - **[Linux Mint > Download > All versions](https://linuxmint.com/download_all.php)**
   - **Linux Mint 22 "Wilma" - MATE (64-bit)**: `linuxmint-22-mate-64bit`
2. **Oracle VM VirtualBox マネージャー**を起動する。
   1. **仮想マシン > 新規**を開く。
   2. **Virtual machine Name and Operating System**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **名前**: `Linux Mint 22 MATE 64-bit`
      - **Folder**:
        - `X:\VirtualBox VMs` # 外付けSSDのドライブを`X`とし、そこに構築する場合
      - **ISO Image**: `X:\Downloads\linuxmint-22-mate-64bit.iso`
   3. **Unattended Guest OS Install Setup**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **Username**: `rsl`
      - **Password**: 【任意のパスワード】
      - **Repeat Password**: 【パスワードの確認】
      - **Hostname**: `recsyslab-mint`
      - **Domain Name**: `localhost`
      - **Guest Addtions**: `チェック`
      - **Guest Addtions ISO**: `C:\Program Files\Oracle\VirtualBox\VBoxGuestAdditions.iso`
   4. **Hardware**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **メインメモリー**: （デフォルト）
      - **Processors**: （デフォルト）
   5. **Virtual Hard dis**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **Create a Virtual Hard Disk Now**: `320 GB` # 自分の空き容量に合わせる
      - **Pre-allocate Full Size**: `非チェック`
   6. **完了**ボタンをクリックする。
3. **GNU GRUB**が起動するので、`Start Linux Mint 21.2 MATE 64-bit`を選択し、**Enter**キーを押す。

#### 参考
- 鶴長鎮一，『作りながら学ぶ Webシステムの教科書』，日経BP，2023．
