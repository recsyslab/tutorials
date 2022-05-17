1. 09_JavaScriptファイルの準備までは、[webgame](../webgame/)と同様
   - プロジェクト名: `recsys_django`
   - アプリケーション名: `sushi_recommender`
   - URL: http://localhost:8000/sushi_recommender/

# データベース環境の構築
```pgsql
postgres=# CREATE DATABASE sushi_recommender ENCODING 'UTF8';
postgres=# \l
postgres=# \c sushi_recommender
sushi_recommender=# \dt
```

# データベースの設定

リスト1: `recsys_django/recsys_django/settings.py`
```py
...（略）...
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',	# 修正
        'NAME': 'webgame',							# 修正
        'USER': os.environ.get('DB_USER'),			# 追加
        'PASSWORD': os.environ.get('DB_PASSWORD'),	# 追加
        'HOST': '',									# 追加
        'PORT': '',									# 追加
    }
}
...（略）...
```

# マイグレーションの実行
```bash
export DB_USER=【ユーザ名】
export DB_PASSWORD=【パスワード】
python manage.py migrate
```

```bash
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK

Process finished with exit code 0
```

```pgsql
sushi_recommender=# \dt
                  List of relations
 Schema |            Name            | Type  | Owner 
--------+----------------------------+-------+-------
 public | auth_group                 | table | rsl
 public | auth_group_permissions     | table | rsl
 public | auth_permission            | table | rsl
 public | auth_user                  | table | rsl
 public | auth_user_groups           | table | rsl
 public | auth_user_user_permissions | table | rsl
 public | django_admin_log           | table | rsl
 public | django_content_type        | table | rsl
 public | django_migrations          | table | rsl
 public | django_session             | table | rsl
(10 rows)
```

# 管理サイト
```bash
export DB_USER=【ユーザ名】
export DB_PASSWORD=【パスワード】
python manage.py createsuperuser
----
ユーザー名: admin
メールアドレス: admin@recsys-django.org
Password: 
Password (again): 
----
```

管理サイトにアクセス
http://localhost:8000/admin/


# モデルの定義

| カラム名 | 説明 | データ型 | 制約 |
| --- | --- | --- | --- |
| sushi_id | 寿司ID | INT	| PRIMARY KEY |
| sushi_name | 寿司名 | VARCHAR(20)	| NOT NULL |
| category_id | カテゴリID | INT | NOT NULL |

| カラム名 | 説明 | データ型 | 制約 |
| --- | --- | --- | --- |
| category_id | カテゴリID | INT	| PRIMARY KEY |
| category_name | カテゴリ名 | VARCHAR(20)	| NOT NULL |



