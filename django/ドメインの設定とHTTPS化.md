# ドメインの設定とHTTPS化

## ドメインの設定

### ドメインの取得
1. ConoHaコントロールパネルから、**ドメイン > ドメイン**を開く。
   1. **ドメイン取得**ボタンをクリックする。
   2. ドメイン名に希望のドメイン名を入力し検索する。
   3. 希望のドメイン名を選択し、**カートに追加**ボタンをクリックする。
   4. 支払処理にすすむ。

※ここでは、`recsyslab-ex.org`というドメインを取得したとする。

### DNSの設定
1. ConoHaコントロールパネルから、**DNS**を開く。
   1. **+ドメイン**ボタンをクリックする。
   2. 取得したドメイン名を入力し、**保存**ボタンをクリックする。
   3. **ドメインリスト**にドメインが追加される。
   4. **ドメインリスト**内のドメイン名の部分をクリックし、DNSレコード一覧を表示する。
   5. **編集**アイコンをクリックする。
   6. 下記のように設定する。
      - `A, @, 3600, ＊＊＊.＊＊＊.＊＊＊.＊＊＊`
      - `A, www, 3600, ＊＊＊.＊＊＊.＊＊＊.＊＊＊`
      - `A, rsl＊＊＊, 3600, ＊＊＊.＊＊＊.＊＊＊.＊＊＊`
      - `CNAME, ftp, 3600, ＊＊＊.＊＊＊.＊＊＊.＊＊＊`
      - ※`＊＊＊.＊＊＊.＊＊＊.＊＊＊`はドメイン設定対象のサーバーのIPアドレス。別のサーバのIPアドレスを指定することも可能。
     
### ドメインでのアクセス確認
1. ドメイン設定が反映された後、下記にアクセスし、Webページが表示されれば正常にドメインが設定されている。
   - ※ドメインの設定が反映されるまで数時間かかる。
   - `http://recsyslab-ex.org/`
   - `http://www.recsyslab-ex.org/`
   - `http://rsl＊＊＊.recsyslab-ex.org/`

#### 参考
1. [サーバーにドメインの設定を追加する｜ConoHa WINGサポート](https://support.conoha.jp/w/adddomain/)
2. [DNSを使う｜ConoHa VPSサポート](https://support.conoha.jp/v/dns/)

## HTTPS化

### CSRの生成（クライアント側）
```bash
$ openssl genrsa -aes256 -out ssl_www_recsyslab-ex.pem.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
......+++++
..........................................................................+++++
e is 65537 (0x010001)
Enter pass phrase for ssl_www_recsyslab-ex.pem.key:【パスフレーズ】
Verifying - Enter pass phrase for ssl_www_recsyslab-ex.pem.key:【パスフレーズ】
$ openssl req -new -key ssl_www_recsyslab-ex.pem.key -out ssl_www_recsyslab-ex.csr
Enter pass phrase for ssl_www_recsyslab-ex.pem.key:【パスフレーズ】
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:JP
State or Province Name (full name) [Some-State]:Shiga
Locality Name (eg, city) []:Otsu
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Ryukoku University
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:www.recsyslab-ex.org
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
$ cat ssl_www_recsyslab-ex.csr
```

#### 参考
1. [SSLサーバー証明書の申し込みに必要なCSRの作り方 | ニアの技術研究所](https://chronoir.net/make-csr-for-ssl/)

### SSLの設定
1. ConoHaコントロールパネルから、**セキュリティ > SSL**を開く。
   1. **SSL**ボタンをクリックする。
   2. 下記を設定する。
      - **プラン**: `アルファSSL`
      - **取得形式**: `新規取得`
      - **契約期間**: `1年`
      - **デュアルアクセス**: `有効` ※`www`以外のサブドメインを設定する場合は`無効`にする。
      - **認証方法**: `DNS認証(推奨)`
   3. **次へ**ボタンをクリックする。
   4. **CSR貼付**フォームに**CSRの生成**で生成した`ssl_www_recsyslab-ex.csr`の内容をそのまま貼り付ける。
   5. **CSR内容確認**ボタンをクリックする。
   6. CSR解析結果が正常に表示されれば、**次へ**ボタンをクリックする。
   7. 申請担当者情報を入力し、**次へ**ボタンをクリックする。
   8. 申請内容を確認し、**決定**ボタンをクリックする。
   9. 支払確認をし、**決定**ボタンをクリックする。
   10. 認証設定を行うFQDNを選択し、それぞれ**認証先FQDN**と**認証ID**を取得する。
       - **認証先FQDN**: `recsyslab-ex.org`
       - **認証ID**: `globalsign-domain-verification=＊`
       - **認証先FQDN**: `www.recsyslab-ex.org`
       - **認証ID**: `globalsign-domain-verification=＊`
2. ブラウザの新しいタブで、ConoHaコントロールパネルから、**DNS**を開く。
   1. **ドメインリスト**から、設定先ドメイン名の部分をクリックし、DNSレコード一覧を表示する。
   2. **編集**アイコンをクリックする。
   3. 取得した**認証先FQDN**と**認証ID**を基に、それぞれ下記のDNSレコードを追加する。
       - `TXT, recsyslab-ex.org, 3600, globalsign-domain-verification=＊`
       - `TXT, www.recsyslab-ex.org, 3600, globalsign-domain-verification=＊`
3. 設定先ドメインが外部からアクセスできることを確認する。
4. **セキュリティ > SSL**のタブに戻る。
   1. 各設定先ドメインを選択し、それぞれで**認証依頼**ボタンをクリックする。
      - ※設定が反映されるまでに数時間程度かかる。認証に失敗した場合は、数時間後に再度認証依頼を行う。
   2. 証明書名が表示されれば認証完了。
   3. **SSL TOP**ボタンをクリックする。
   4. **SSLサーバー証明書リスト**に認証された証明書が表示されていることを確認する。

※ワイルドカード証明書には対応していないため、サブドメインごとにコモンネームを設定した証明書が必要。例えば、`rsl＊＊＊`のサブドメインに対応させたいときは、**Common Name**を`rsl＊＊＊.recsyslab-ex.org`としたCSRを生成し、それを使って認証を受ける必要がある。**デュアルアクセス**を`無効`にすることに注意する。

### Certbotパッケージのインストール（サーバ側）
```bash
rsl@＊:~$ sudo apt install certbot python3-certbot-nginx
rsl@＊:~$ sudo certbot --nginx -d recsyslab-ex.org -d www.recsyslab-ex.org
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): 【メールアドレス】

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.3-September-21-2022.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.
Requesting a certificate for recsyslab-ex.org and www.recsyslab-ex.org

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/recsyslab-ex.org/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/recsyslab-ex.org/privkey.pem
This certificate expires on 2024-01-07.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for recsyslab-ex.org to /etc/nginx/sites-enabled/default
Successfully deployed certificate for www.recsyslab-ex.org to /etc/nginx/sites-enabled/default
Congratulations! You have successfully enabled HTTPS on https://recsyslab-ex.org and https://www.recsyslab-ex.org

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

### HTTPSアクセスの動作確認
1. 下記にアクセスし、Webページが表示されれば正常にHTTPS化できている。
   - `https://recsyslab-ex.org/`
   - `https://www.recsyslab-ex.org/`
2. 下記にはアクセスできないことを確認する。
   - `http://recsyslab-ex.org/`
   - `http://www.recsyslab-ex.org/`

#### 参考
1. [nginxでWebサーバーを構築する（Ubuntu編）- マルチドメイン・HTTPS化の設定も解説 - アナグマのモノローグ](https://monologu.com/nginx-ubuntu/)
