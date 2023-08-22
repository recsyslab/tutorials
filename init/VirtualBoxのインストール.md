# VirtualBoxのインストール

## VirtualBoxのインストール

1. 下記からVirtualBoxをダウンロードする。
   - **[Downloads – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)**
   - **VirtualBox 7.＊.＊＊ platform packages > Windows hosts**: `VirtualBox-7.＊.＊＊-＊-Win.exe`
2. `VirtualBox-7.＊.＊＊-＊-Win.exe`を実行する。
   - ダイアログにしたがって任意に設定し、**Install**ボタンをクリックする。
   - インストールが完了したら、**Finish**ボタンをクリックする。

## BIOS設定

仮想マシンの作成において、バージョンの選択に`Ubuntu (64-bit)`が表示されない場合は、事前にこのBIOS設定が必要になる。

HP ProBook 430 G3, G5の場合
1. PC起動時に**F12**キーを押し続け、BIOS画面に入る。
   1. **Network (PXE) Boot Menu**で**Esc**を選択する。
   2. **Startup Menu > BIOS Setup**を選択する。
   3. **Advanced > System Options**を開き、下記を設定する。
      - **Virtualization Technology (VTx)**: `チェック`
   4. **Main > Save Changes and Exit**を選択する。
      - **Save Changes**: `Yes`

## VirtualBox上でのLinux Mint 20.3 MATE 64-bitの仮想マシンの構築
1. 下記からLinux Mint 20.3のISOファイルをダウンロードする。
   - **[Linux Mint > Download > All versions](https://linuxmint.com/download_all.php)**
   - **Linux Mint 20.3 "Una" - MATE (64-bit)**: `linuxmint-20.3-mate-64bit.iso`
2. **Oracle VM VirtualBox マネージャー**を起動する。
   1. **仮想マシン > 新規**を開く。
   2. **仮想マシンの作成**ダイアログで下記を設定する。
      - **名前**: `Linux Mint 20.3 MATE 64-bit`
      - **マシンフォルダー**: `C:\Users\【ユーザアカウント名】\VirtualBox VMs`
      - **タイプ**: `Linux`
      - **バージョン**: `Ubuntu (64-bit)`
        - リスト中に`64-bit`が表示されない場合は、上記のBIOS設定を確認する。
      - **メモリーサイズ**: （デフォルト）
        - 後で設定する。
      - **ハードディスク**: `仮想ハードディスクを作成する`
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
