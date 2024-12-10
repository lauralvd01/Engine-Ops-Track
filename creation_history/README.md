# Creating a Django project

[Step-by-step guide](https://medium.com/@nick_damoulakis/a-step-by-step-integration-of-django-and-vue-js-8b70fe60aec3)

> Django is a free, open-source Python web framework that’s designed for developing and managing complex, secure, and database-driven websites, and Vue.js is an open-source JavaScript framework designed for creating dynamic user interfaces.

>Integrating Django with Vue.js allows developers to harness the full power of Django’s robust backend capabilities alongside Vue.js’s responsive frontend features. This combination greatly enhances web development efficiency by streamlining the process of building complex, interactive web applications that are both highly scalable and easily maintainable.

### Requirements

- Python (here Python312) & Django

*As an administrator run*

    python.exe -m pip install --upgrade pip
    pip install django
    django-admin startproject engine_backend


#  Using MySQL for the database

[Step-by-step guide](https://dev.to/sm0ke/how-to-use-mysql-with-django-for-beginners-2ni0)

- ## Install the MySql Server

[MySQL Windows Installer](https://dev.mysql.com/downloads/installer/) : *mysql-installer-web-community-8.0.40.0.msi*

Choose and register somewhere a MySQL Root Password

Create a user as DB admin (with all rights), with its password : *lauralvd01*

- ## Install the Mysql Python driver

Used by Django to connect and communicate

    pip install mysqlclient

Always add the packages needed to `engine_backend/requirements.txt`

- ## Create the Mysql database

Connect to mysql, using MySQL 8.0 Command Line Cli and MySQL Root Password

    CREATE DATABASE engine_db;

- ## Update Django settings

Create a `.env` at the root of the backend app (add it to `.gitignore`)

In `engine_backend/.env` enter personal settings :

    # Django settings
    SECRET_KEY=your-secret-key
    DEBUG="True"
    ALLOWED_HOSTS="localhost 127.0.0.1 [::1]""

    # MySQL settings
    MYSQL_ROOT_PASSWORD=your-mysql-root-password
    MYSQL_DATABASE=your-mysql-database
    MYSQL_USER=your-mysql-username
    MYSQL_PASSWORD=your-mysql-password
    MYSQL_HOST="localhost"
    MYSQL_PORT="3306"

Import decouple and config Django settings :

    pip install python-dotenv

In `engine_backend/engine_backend/settings.py`

    ...

    from pathlib import Path
    from dotenv import load_dotenv
    load_dotenv()
    import os

    ...

    # SECURITY WARNING: keep the secret key used in production secret!
    SECRET_KEY = os.environ.get('SECRET_KEY')

    # SECURITY WARNING: don't run with debug turned on in production!
    DEBUG = os.environ.get('DEBUG')

    ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS')

    ...

    # Database
    # https://docs.djangoproject.com/en/5.1/ref/settings/#databases

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': os.environ.get('MYSQL_DATABASE'),
            'USER': os.environ.get('MYSQL_USER'),
            'PASSWORD': os.environ.get('MYSQL_PASSWORD'),
            'HOST': os.environ.get('MYSQL_HOST'),
            'PORT': os.environ.get('MYSQL_PORT')
        }
    }

Update the `SECRET_KEY` using

    python ./engine_backend/manage.py shell

    from django.core.management.utils import get_random_secret_key
    print(get_random_secret_key())

- ## Execute Django migration and create the project tables

    python engine_backend/manage.py makemigrations
    python engine_backend/manage.py migrate

Start the project and check that the installation work on : [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

    python engine_backend/manage.py runserver

