# URLディスパッチャ

URLディスパッチャは，`URLconf`に記述された`urlpatterns`に基づき，要求されたURLに対応したビュー関数を呼び出す役割を担います．それでは，`http://localhost:8000/touch/`にアクセスされたとき，どのような流れでインデックスページが表示されたのか，リストを追いながら見ていきましょう．

`webgame/settings.py`
```python
...（略）...
ROOT_URLCONF = 'webgame.urls'
...（略）...
```

まず，`webgame/settings.py`には，`ROOT_URLCONF = 'webgame.urls'`という記述があります．そのため，`http://localhost:8000/touch/`にアクセスされると，最初に`webgame/urls.py`の`urlpatterns`を見にいきます．

`webgame/urls.py`
```python
...（略）...
urlpatterns = [
    path('admin/', admin.site.urls),
    path('touch/', include('touch.urls')),
]
```

この`urlpatterns`には`path()`関数があります．この`path()`関数の第1引数がURLパターンを表しています．要求されたURLと一致するまで，このURLパターンを上から順にマッチングしていきます．ただし，このURLパターンにはドメイン名（今回の場合，`http://localhost:8000/`の部分）を含みません．つまり，二つ目のURLパターンが`touch/`に一致します．

一致するURLパターンを見つけると，次は`path()`関数の第2引数に指定されたビュー関数を呼び出します．ただし，ここでは`include('touch.urls')`となっています．`include()`関数は，他の`URLconf`を参照することを表します．`'touch.urls'`と記述されていますので，今度は`touch/urls.py`の`urlpatterns`を見にいくことになります．この際，要求されたURLの中で既に一致した部分は取り除かれたうえで，次の`URLconf`に渡されます．つまり，`'touch/'`の部分は既に一致していますので，これを除いた部分`''`（空文字列）が`touch/urls.py`の`URLconf`に渡されます．

`touch/urls.py`
```python
...（略）...
app_name = 'touch'
urlpatterns = [
    # ex: touch/
    path('', views.index, name='index'),
]
```

要求されたURLを`''`として，同様にURLパターンをマッチングしていきます．ここでは，`''`のURLパターンに一致しますので，第2引数の`views.index`が呼ばれることになります．ここで，`views.index`は`IndexView`クラスに記述した`index = IndexView.as_view()`を指します．今回の例では，インデックスビューを`IndexView`というクラスで定義しています．`as_view()`は，そのクラスベースのビューをビュー関数化するためのメソッドです．そして，この`IndexView`クラスの`get()`メソッドが呼び出されます．

`touch/views.py`
```python
...（略）...
class IndexView(View):
    """
    インデックスビュー
    """
    def get(self, request, *args, **kwargs):
        # テンプレートのレンダリング
        return render(request, 'touch/index.html')


index = IndexView.as_view()
```

`touch/urls.py`の`app_name`および`path()`関数の引数`name`はURLの逆引きに用いられます．`app_name`にはアプリケーション名を設定しておくと良いでしょう．また，引数`name`は`urls.py`内でユニークな値に設定しておく必要があります．簡単には関数名を設定しておくと良いでしょう．

`path()`関数，`include()`関数，`View.as_view()`メソッドの詳細については，それぞれ文献[1][2][3]を参照してください．URLディスパッチャと`URLconf`の詳細については，文献[4][5]を参照してください．

#### 参考
1. [django.urls functions for use in URLconfs | Django ドキュメント | Django # path()](https://docs.djangoproject.com/ja/3.2/ref/urls/#django.urls.path)
1. [django.urls functions for use in URLconfs | Django ドキュメント | Django # include()](https://docs.djangoproject.com/ja/3.2/ref/urls/#django.urls.include)
1. [Base views | Django ドキュメント | Django # View.as_view()](https://docs.djangoproject.com/ja/3.2/ref/class-based-views/base/#django.views.generic.base.View.as_view)
1. [URL ディスパッチャ | Django ドキュメント | Django](https://docs.djangoproject.com/ja/3.2/topics/http/urls/)
1. 現場で使える Django の教科書《基礎編》 # 第4章 URLディスパッチャとURLconf
