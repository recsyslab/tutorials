# VirtualBoxのインストール

## VirtualBoxのインストール

1. 下記からVirtualBoxをダウンロードする。
   - [VirtualBox > Downloads](https://www.virtualbox.org/wiki/Downloads) > VirtualBox 6.＊.＊＊ platform packages > Windows hosts
   - `VirtualBox-6.＊.＊＊-＊-Win.exe`
2. `VirtualBox-6.＊.＊＊-＊-Win.exe`を実行する。
   - ダイアログにしたがって任意に設定し、*Install*ボタンをクリックする。
   - インストールが完了したら、[Finish]ボタンをクリックする。

## BIOS設定

仮想マシンの作成において、バージョンの選択に`Ubuntu (64-bit)`が表示されない場合は、事前にこのBIOS設定が必要になる。

HP ProBook 430 G3, G5の場合
1. PC起動時に[F12]キーを押し続け、BIOS画面に入る。
   1. [Network (PXE) Boot Menu]で[Esc]を選択する。
   2. [Startup Menu] – [BIOS Setup (F10)]を選択する。
   3. *Advanced – System Options*を開き、下記を設定する。
      - *Virtualization Technology (VTx)*: チェック
   4. [Main] – [Save Changes and Exit]を選択する。
      - Save Changes: Yes
