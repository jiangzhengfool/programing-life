安装Django
`pip install django	`

创建项目
`django-admin startproject hello_djang`
`python manage.py runserver`

创建app
~~~
cd hello_django
django-admin startapp hello
~~~
打开配置文件，找到`INSTALLED_APPS`，然后把我们创建的app配置添加进去，这样django才能使用我们的app。

~~~
INSTALLED_APPS = [
    'hello.apps.HelloConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
~~~
命令生成数据库表
`python manage.py migrate`

修改默认的数据库

创建管理员账户
python manage.py createsuperuser

https://blog.csdn.net/u011054333/article/details/78767724

