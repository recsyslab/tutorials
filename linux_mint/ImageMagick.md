# ImageMagick

## インストール
```bash
sudo apt install imagemagick
```

## 設定ファイルの準備
`policy.xml`
```bash
ls /etc/ImageMagick-6/
less /etc/ImageMagick-6/policy.xml
sudo cp /etc/ImageMagick-6/policy.xml /etc/ImageMagick-6/policy.xml-org
ls /etc/ImageMagick-6/
```

## convertのバージョン確認
```bash
convert -version
```

## convertの設定
```bash
less /etc/ImageMagick-6/policy.xml
sudo vi /etc/ImageMagick-6/policy.xml
```

下記の箇所を修正する。

修正前
```bash
  <policy domain="coder" rights="none" pattern="PDF" />
```

修正後
```bash
  <policy domain="coder" rights="read | write" pattern="PDF" />
```

```bash
less /etc/ImageMagick-6/policy.xml
diff /etc/ImageMagick-6/policy.xml-org /etc/ImageMagick-6/policy.xml
```

## convertの実行
convert [元画像ファイル名] [変換後画像ファイル名]

#### 参考
- しろかい！, [超簡単！TeXにpdf形式の図を挿入する方法](http://shirokai.hatenablog.com/entry/tex-figure-pdf)
