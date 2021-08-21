# 利用方法

## 前提
* dockerがインストールされていること
* gitがインストールされていること

## 1. リポジトリをPullしてローカル環境構築

```
git clone https://github.com/TodoONada/django-nginx-postgres.git
```

## 2. doekcr-composeでイメージ作成
```
cd django-nginx-postgres
docker-compose build --no-cache
```

## 3. Djangoプロジェクトの作成

```
docker-compose run python3 django-admin startproject project .
```

## 5. settingsを修正

```./src/project/setting```の以下を修正。

```
import os

...

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_DB', 'postgres'),
        'USER': os.environ.get('POSTGRES_USER', 'postgres'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD', 'postgres'),
        'HOST': 'db',
        'PORT': os.environ.get('POSTGRES_PORT', 5432),
    }
}
```

サンプルは```./python3/settings_sample.py````を参照

## 6. データベースのマイグレーション

```
docker-compose run python3 python manage.py makemigrations
docker-compose run python3 python manage.py migrate
```

## 7. アプリケーションの作成

```
docker-compose run python3 python manage.py startapp <new_application>
```

