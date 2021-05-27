# VirtualBoxの環境設定

## 環境設定
1. **Oracle VM VirtualBox マネージャー**を起動する。
   1. **ファイル > 環境設定**を開く。
   2. 下記を設定する。
      - **入力 > 仮想マシン > ホストキーの組み合わせ**: `Left Windows`

## Guest Additionsのインストール
1. **Oracle VM VirtualBox マネージャー**を起動する。
   1. 仮想マシンを選択し、**起動**ボタンをクリックする。
      1. Linuxにログインする。
      2. Virtual Boxのメニューから**デバイス > Guest Additions CD イメージの挿入**を開く。
      3. デスクトップに現れた`VBox_GAs_6.＊.＊＊`を開く。
      4. 開かれたフォルダ内の`autorun.sh`を実行する。
         1. **実行**ボタンをクリックする。
            - （1分程度）
         2. 「Press Return to close this window」と表示されたら**ENTER**キーを押す。
      5. `VBox_GAs_6.＊.＊＊`を右クリックし、**取り出す**を選択する。
      6. Linuxを再起動する。
         - ウィンドウサイズを変更したときに、Linuxの画面が自動的にリサイズされることを確認する。
         - リサイズされない場合は、Virtual Boxのメニューから**表示 > ゲストOSの画面を自動リサイズ**を選択する。
