# JavaScriptファイルの準備

ユーザ入力の受付やイベント処理，画面への出力などユーザインタフェースに関わる部分は主にJavaScriptを用います．ここでは，<a href="https://jquery.com/">jQuery</a>とよばれるJavaScriptライブラリを参照します．jQueryを用いることで，JavaScriptコードをより簡単に記述できるようになります．本チュートリアルでは，`jquery-3.5.1.min.js`を用います．`jquery-3.5.1.min.js`を参照するために`webgame/touch/templates/touch/index.html`の`{# --- js --- #}`の部分に下記コードを追加してください．

リスト1: `webgame/touch/templates/touch/index.html`
```html
...（略）...
    {# --- js --- #}
    <script src="https://code.jquery.com/jquery-3.5.1.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script> <!-- 追加 -->
...（略）...
```

ここでは，CDN (Content Delivery Network) を用いてjQueryを参照しています．上記のコードは，<a href="https://code.jquery.com/">jQuery CDN</a>から確認できます．このようにCDNを利用することで，手軽にjQueryを利用することができます．

つづいて，JavaScriptのソースファイルを独自に作成します．JavaScriptのソースファイルやCSSファイル，画像ファイルなどは静的ファイルとよばれます．まず，静的ファイルの配置ディレクトリを設定します．`webgame/webgame/settings.py`に下記のコードを追加してください．

リスト2: `webgame/webgame/settings.py`
```py
...（略）...
STATIC_URL = '/static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]       # 追加
...（略）...
```

ベースディレクトリ直下に，`static`ディレクトリを作成してください．その中に`js`ディレクトリを作成してください．つまり，下記のディレクトリを用意します．

`webgame/touch/static/js/`

独自に作成したJavaScriptのソースファイルはこのディレクトリの中に置くことにします．ここに，メインのJavaScriptファイルである`main.js`を作成します．そして，`main.js`を参照するために，`webgame/touch/templates/touch/index.html`の`{# --- js --- #}`の部分に，先ほどjQueryを参照するために追加したコードの下に下記のコードを追加してください．

リスト3: `webgame/touch/templates/touch/index.html`
```html
{% load static %}       <!-- 追加 -->
<!DOCTYPE html>
...（略）...
    <script src="https://code.jquery.com/jquery-3.5.1.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
    <script type="text/javascript" src="{% static 'js/main.js' %}"></script>                                                                                    <!-- 追加 -->
...（略）...
```

5行目で`main.js`のJavaScriptファイルを参照しています．このように，`static`ディレクトリに配置されたファイルを参照するには，`{% static '【static以下のパス】' %}`という形式で`static`タグを利用します．`static`タグを利用するには1行目の`{% load static %}`という記述が必要ですので忘れないようにしてください．

テンプレートタグについては文献[2]を参照してください．

#### 参考
1. 現場で使える Django の教科書《基礎編》 # 10.4 静的ファイル関連の設定
1. 現場で使える Django の教科書《基礎編》 # 7.4 テンプレートタグ
