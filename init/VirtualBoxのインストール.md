# VirtualBoxのインストール

## VirtualBoxのインストール

1. 下記からVirtualBoxをダウンロードする。
   - **[Downloads – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)**
   - **VirtualBox 7.＊.＊＊ platform packages > Windows hosts**: `VirtualBox-7.＊.＊＊-＊-Win.exe`
2. `VirtualBox-7.＊.＊＊-＊-Win.exe`を実行する。
   - ダイアログにしたがって任意に設定し、**Install**ボタンをクリックする。
   - インストールが完了したら、**Finish**ボタンをクリックする。

## VirtualBox上でのLinux Mint 21.2 MATE 64-bitの仮想マシンの構築
1. 外付けSSDを接続する。以降、外付けSSDのドライブを `D` とする。
1. 下記からLinux Mint 21.2のISOファイルをダウンロードする。
   - **[Linux Mint > Download > All versions](https://linuxmint.com/download_all.php)**
   - **Linux Mint 21.2 "Victoria" - MATE (64-bit)**: `linuxmint-21.2-mate-64bit.iso`
2. **Oracle VM VirtualBox マネージャー**を起動する。
   1. **仮想マシン > 新規**を開く。
   2. **Virtual machine Name and Operating System**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **名前**: `Linux Mint 21.2 MATE 64-bit`
      - **Folder**: `D:\VirtualBox VMs` # 外付けSSDのドライブ
      - **ISO Image**: `【ディレクトリ】/linuxmint-21.2-mate-64bit.iso` # ダウンロードしたISOファイル
   3. **Unattended Guest OS Install Setup**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **Username**: `rsl`
      - **Password**: （任意のパスワード）
      - **Repeat Password**: （パスワードの確認）
      - **Hostname**: `recsyslab-mint`
      - **Domain Name**: `localhost`
      - **Guest Addtions**: `チェック`
      - **Guest Addtions ISO**: `C:\Program Files\Oracle\VirtualBox\VBoxGuestAdditions.iso`
   4. **Hardware**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **メインメモリー**: （緑色の範囲内での最大値）
      - **Processors**: （緑色の範囲内での最大値）
   5. **Virtual Hard dis**ダイアログで下記を設定し、**次へ**ボタンをクリックする。
      - **Create a Virtual Hard Disk Now**: `320 GB`
      - **Pre-allocate Full Size**: `非チェック`
   6. **完了**ボタンをクリックする。
3. **GNU GRUB**が起動するので、`Start Linux Mint 21.2 MATE 64-bit`を選択し、**Enter**キーを押す。
4. [Linux Mintのインストール](Linux_Mintのインストール.md)の手順にしたがって、Linux Mintをインストールする。

（9/2ここまで完了）
    3. **作成**ボタンをクリックする。
    4. **仮想ハードディスクの作成**ダイアログで下記を設定する。
       - **ハードディスクのファイルタイプ**: `VDI (VirtualBox Disk Image)`
       - **物理ハードディスクにあるストレージ**: `可変サイズ`
       - **ファイルの場所**: `C:\Users\【ユーザアカウント名】\VirtualBox VMs\Linux Mint 20.3 MATE 64-bit`
       - **仮想ハードディスクのサイズ**: （可能な限り大きく設定する（目安：ディスク容量の50%程度））
    5. **作成**ボタンをクリックする。
    6. 作成した仮想マシン**Linux Mint 20.3 MATE 64-bit**を選択し、**設定**ボタンをクリックする。
       1. **設定**ダイアログで下記を設定する。
          - **一般 > 高度 > クリップボードの共有**: `双方向`
          - **一般 > 高度 > ドラッグ&ドロップ**: `双方向`
          - **システム > マザーボード > メインメモリー**: （選択可能な範囲内での最大値（無効な設定エラーが出る手前））
          - **システム > プロセッサー > プロセッサー数**: （選択可能な範囲内での最大値（無効な設定エラーが出る手前））
          - **ディスプレイ > スクリーン > ビデオメモリー**: `128MB`
            - ディスプレイ数やビデオメモリを多くしすぎると仮想マシンが起動しなかったり、画面がおかしくなったりすることがある。仮想マシンが正常に起動する範囲で、それぞれ設定すると良い。
          - **ディスプレイ > スクリーン > ディスプレイ数**: `2`
          - **ディスプレイ > スクリーン > グラフィックスコントローラー**: `VMSVGA`
            - 他のコントローラーを選択すると、マルチディスプレイが使えないことがある。
            - ENVYの場合は、VBoxVGAを選択すると無効になるので、VMSVGAを選択する。
            - ProBook 430の場合は、VBoxVGA（VMSVGA）？
          - **ディスプレイ > スクリーン > 3Dアクセラレーションを有効化**: `チェック`
          - **ディスプレイ > レコーディング > レコーディングを有効化**: `非チェック`
          - **共有フォルダー**
            - 右の**共有フォルダーの追加**ボタンをクリックし、下記を設定する。
              - **フォルダーのパス**: `C:`
              - **フォルダー名**: `C_DRIVE`
              - **自動マウント**: `チェック`
              - **マウントポイント**: `/mnt/c/`
            - Dドライブに外付けSSDを接続する場合、右の**共有フォルダーの追加**ボタンをクリックし、下記を設定する。
              - **フォルダーのパス**: `D:`
              - **フォルダー名**: `D_DRIVE`
              - **自動マウント**: `チェック`
              - **マウントポイント**: `/mnt/d/`
          - **ストレージ**
            - **ストレージデバイス**から空を選択する。
            - **属性 > 光学ドライブ**の右のアイコンをクリックし、**ディスクファイルを選択**（または**仮想光学ディスクファイルを選択**）をクリックする。
                  - ダウンロードした`linuxmint-20.3-mate-64bit.iso`を選択する。
       2. **OK**ボタンをクリックする。
     7. 設定した仮想マシン``Linux Mint 20.3 MATE 64-bit``を選択し、**起動**ボタンをクリックする。
     8. [Linux Mintのインストール](Linux_Mintのインストール.md)の手順にしたがって、Linux Mintをインストールする。

#### 参考
- PC設定のカルマ, [VirtualBoxでOS間のクリップボードを共有する方法](https://pc-karuma.net/virtualbox-clipboard-share/)
- 情報科学屋さんを目指す人のメモ, [VirtualBox：仮想マシンの作成で64bit OSを選択可能にする方法](https://did2memo.net/2015/07/10/virtualbox-64-bit-os/)
