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

リスト: `recsys_django/recsys_django/settings.py`
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
$ export DB_USER=【ユーザ名】
$ export DB_PASSWORD=【パスワード】
$ python manage.py migrate
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
$ export DB_USER=【ユーザ名】
$ export DB_PASSWORD=【パスワード】
$ python manage.py createsuperuser
----
ユーザー名: admin
メールアドレス: admin@recsys-django.org
Password: 
Password (again): 
----
```

管理サイトにアクセス

http://localhost:8000/admin/

# ユーザの追加
Alice, Bruno, Chiara, Dhruv, Emiを追加


# モデルの定義

| カラム名 | 説明 | データ型 | 制約 |
| --- | --- | --- | --- |
| category_id | カテゴリID | INT	| PRIMARY KEY |
| category_name | カテゴリ名 | VARCHAR(20)	| NOT NULL |

| カラム名 | 説明 | データ型 | 制約 |
| --- | --- | --- | --- |
| sushi_id | 寿司ID | INT	| PRIMARY KEY |
| sushi_name | 寿司名 | VARCHAR(20)	| NOT NULL |
| category_id | カテゴリID | INT | FOREIGN KEY(category.category_id) |

| カラム名 | 説明 | データ型 | 制約 |
| --- | --- | --- | --- |
| id | ID | INT	| PRIMARY KEY |
| user_id | ユーザID | INT	| FOREIGN KEY(auth_user.id) |
| sushi_id | 寿司ID | INT | FOREIGN KEY(sushi.sushi_id) |
| rating | 評価値 | INT | NOT NULL |
| updated_at | 更新日時 | TIMESTAMP | NOT NULL |


リスト: `recsys_django/sushi_recommender/models.py`
```py
from django.db import models
from django.contrib.auth.models import User
from django.core.validators import MinValueValidator, MaxValueValidator


class Category(models.Model):
    """カテゴリモデル"""

    class Meta:
        db_table = 'categories'

    category_id = models.IntegerField(verbose_name='カテゴリID', primary_key=True)
    category_name = models.CharField(verbose_name='カテゴリ名', max_length=20)

    def __str__(self):
        return str(self.category_name)


class Sushi(models.Model):
    """寿司モデル"""

    class Meta:
        db_table = 'sushi'

    sushi_id = models.IntegerField(verbose_name='寿司ID', primary_key=True)
    sushi_name = models.CharField(verbose_name='寿司名', max_length=20)
    category = models.ForeignKey(Category, verbose_name='カテゴリID', on_delete=models.PROTECT)

    def __str__(self):
        return str(self.sushi_name) + ', ' + str(self.category.category_name)


class UserSushiRating(models.Model):
    """ユーザ-寿司-評価値モデル"""

    class Meta:
        db_table = 'users_sushi_ratings'

    user = models.ForeignKey(User, on_delete=models.CASCADE)
    sushi = models.ForeignKey(Sushi, on_delete=models.CASCADE)
    rating = models.IntegerField(validators=[MinValueValidator(1), MaxValueValidator(5)])
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return str(self.user.username) + ', ' + str(self.sushi.sushi_name) + ', ' + str(self.rating)
```

```bash
$ python manage.py makemigrations sushi_recommender
Migrations for 'sushi_recommender':
  sushi_recommender/migrations/0001_initial.py
    - Create model Category
    - Create model Sushi
    - Create model UserSushiRating
```

```bash
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, sushi_recommender
Running migrations:
  Applying sushi_recommender.0001_initial... OK
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
 public | categories                 | table | rsl
 public | django_admin_log           | table | rsl
 public | django_content_type        | table | rsl
 public | django_migrations          | table | rsl
 public | django_session             | table | rsl
 public | sushi                      | table | rsl
 public | users_sushi_ratings        | table | rsl
(13 rows)
```

