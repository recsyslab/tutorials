# VirtualBoxのインストール

## VirtualBoxのインストール

1. 下記からVirtualBoxをダウンロードする。
   - **[Downloads – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)**
   - **VirtualBox 7.1.8 > Windows hosts**: `VirtualBox-7.1.8-168469-Win.exe`
   - ※今回構築した仮想マシンを別のPCでも登録したい場合は、VirtualBoxのバージョンを合わせておく必要がある。
2. `VirtualBox-7.1.8-168469-Win.exe`を実行する。
   1. ダイアログにしたがって任意に設定し、**Install**ボタンをクリックする。
   2. インストールが完了したら、**Finish**ボタンをクリックする。

## VirtualBox上でのLinux Mint 22.3 MATE 64-bitの仮想マシンの構築

※外付けSSDのドライブを`X`とし、そこに構築することを想定する。

1. 下記からLinux Mint 22.3のISOファイルをダウンロードし、`X:\Downloads\`ディレクトリに置く。
   - **[Linux Mint > Download > All versions](https://linuxmint.com/download_all.php)**
   - **Linux Mint 22.3 "Zena" - MATE (64-bit)**: `linuxmint-22.3-mate-64bi.iso`
2. **Oracle VM VirtualBox マネージャー**を起動する。
   1. **仮想マシン > 新規**を開く。
   2. **仮想マシンの名前とOS**ダイアログで下記を設定する。
      - **名前とオペレーティングシステム**
        - **名前**: `Linux Mint 22.3 MATE 64-bit`
        - **フォルダー**:
          - `X:\VirtualBox VMs`
        - **ISO イメージ**: `X:\Downloads\linuxmint-22.3-mate-64bit.iso`
        - **タイプ**: `Linux`
        - **Subtype**: `Ubuntu`
        - **バージョン**: `Ubuntu (64-bit)`
        - **自動インストールをスキップ**: `チェック`
      - **ハードウェア**
        - **メインメモリー**: （デフォルト）
        - **プロセッサー数**: （デフォルト）
      - **ハードディスク**
        - **仮想ハードディスクを作成する**: `選択`
        - **ハードディスクファイルの場所**: `X:\VirtualBox VMs\Linux Mint 22.3 MATE 64-bit\Linux Mint 22.3 MATE 64-bit.vdi`
        - **サイズ**: `512.00 GB` # 自分の空き容量に合わせる
        - **ハーディスクのファイルタイプと種類**: `VDI (VirtualBox Disk Image)`
        - **全サイズの事前割当て**: `非チェック`
   3. **完了**ボタンをクリックする。
3. 対象の仮想マシンを選択し、**起動**ボタンをクリックする。
   1. 「Welcome to Linux Mint 22.3 64-bit」と表示されるので、`Start Linux Mint`を選択し、**Enter**キーを押す。

#### 参考

1. 鶴長鎮一，『作りながら学ぶ Webシステムの教科書』，日経BP，2023．
