# セキュリティソフトのインストール

LinuxMint20.1では「Linux Distribution not supported」とエラーが表示される。

## Symantec Endpoint Protection (SEP) のインストール

## ダウンロード
1. 下記RINSのWebページからOSに対応した最新版のSEPインストーラ（dpkg）をダウンロードする。
   - **[RINS Symantec Endpoint Protection関連ドキュメント（学内限定）](https://www.st.ryukoku.ac.jp/security/sep/)**
   - **SEPインストーラ**: `LinuxInstaller`
   - 2021/09/21時点の最新版: 14.3RU2

## インストール
```bash
mv ~/Downloads/LinuxInstaller ~/src/
cd ~/src/
ls -l
sudo chmod u+x LinuxInstaller
ls -l
./LinuxInstaller
```

## バージョンの確認
```bash
/opt/Symantec/symantec_antivirus/sav info --product
```

## 後始末
```bash
cd
mkdir ~/src/sepfiles/log/
mv sep*.log ~/src/sepfiles/log/
rm -f ~/src/SymantecEndpointProtection.zip
```

## 設定
『Symantec AntiVirus for Linux の運用コマンド』を参照

#### 参考
- Symantec, [Installing the Symantec Endpoint Protection client for Linux](https://techdocs.broadcom.com/us/en/symantec-security-software/endpoint-security-and-management/endpoint-protection/all/getting-up-and-running-on-for-the-first-time-v45150512-d43e1033/installing-clients-with-save-package-v16194723-d21e1502/installing-the-client-for-linux-v95193124-d21e2986.html)
- bayanの<del>電波</del>日記, [Symantec AntiVirus for Linux (SAVFL) の sav コマンドのヘルプ](https://bayan.hatenadiary.com/entry/20121127/1354020993)
- [RINS Symantec Endpoint Protection関連ドキュメント](https://www.st.ryukoku.ac.jp/security/sep/)（学内限定）
