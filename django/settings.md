# Djangoプロジェクトの設定

リスト1: `recsys_django/recsys_django/settings.py`
```py
...（略）...
DEBUG = False                                      # 修正

ALLOWED_HOSTS = [os.environ.get('ALLOWED_HOSTS')]  # 修正
...（略）...
STATIC_URL = 'static/'
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)
STATIC_ROOT = '/usr/share/nginx/html/static'    # 追記
MEDIA_ROOT = '/usr/share/nginx/html/media'      # 追記
...（略）...
# ロギング
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,

    # ロガーの設定
    'loggers': {
        # Djangoが利用するロガー
        'django': {
            'handlers': ['file'],
            'level': 'INFO',
        },
        # onlineアプリケーションが利用するロガー
        'online': {
            'handlers': ['file'],
            'level': 'INFO',
        },
    },

    # ハンドラの設定
    'handlers': {
        'file': {
            'level': 'INFO',
            'class': 'logging.handlers.TimedRotatingFileHandler',
            'filename': os.path.join(BASE_DIR, 'logs/django.log'),
            'formatter': 'prod',
            'when': 'D',        # ログローテーション（新しいファイルへの切り替え）間隔の単位（D=日）
            'interval': 1,      # ログローテーション間隔（1日単位）
            'backupCount': 7,   # 保存しておくログファイル数
        },
    },

    # フォーマッタの設定
    'formatters': {
        'prod': {
            'format': '\t'.join([
                '%(asctime)s',
                '[%(levelname)s]',
                '%(pathname)s(Line:%(lineno)d)',
                '%(message)s',
            ])
        },
    },
}
```

