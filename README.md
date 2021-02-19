'''
在 MTV 模式中，
MVC 中的 View 分成了视图 View（展现哪些数据）
和模板 Template（如何展现）两个部分，
而控制器（Controller）这个要素由框架自己来实现了，
我们需要做的就是把 URL 对应到视图 V 就可以了，
通过这样的 URL 配置，系统将一个请求发送到一个合适的视图。
'''

$ pip3 install --user pip  # --user or -U 
$ pip install django==2.2.9
$ pip install ipython mysqlclient

# 创建 Web 项目：
$ django-admin startproject myProject

# 主目录myProject下：
'''
• manage.py 项目的入口文件，在后面的实验中我们会大量使用它来执行一些命令用来创建应用、启动项目、控制数据表迁移等。
• myProject 主目录下的同名子目录，为项目的核心目录，它里面包含配置文件和管理应用的文件。
• myProject/__init__.py 每个子目录都会包含这样一个 __init__.py 文件，它是一个空文件，在需要的时候会引入目录下的对象。
• myProject/settings.py 配置文件，里面包含对数据库的设置项、CSRF Token 的设置项、模板的设置项等全部设置。
• myProject/urls.py 路由控制文件，处理客户端请求，分发到对应的视图函数去处理。
• myProject/wsgi.py 处理请求和响应，我们很少去动它。
'''

# 创建应用：
$ cd myProject
$ python manage.py startapp myApp

# 应用目录myApp下：
'''
• myApp/admin.py 用于控制后台管理的文件，在后面的实验中会用到。
• myApp/apps.py 用于管理应用本身的文件，包括应用的名字如何命名，默认就是 myApp 。
• myApp/__init__.py 空文件，前面已经介绍过。
• myApp/migrations 这是用于记录数据库变更信息的目录，Django 中自带的数据库版本控制功能就体现在这个目录，在学习数据存储时会详细介绍。
• myApp/models.py 创建映射类的文件，熟悉 Flask 的同学一定不陌生。
• myApp/tests.py 编写测试代码的文件。
• myApp/views.py 创建视图函数的文件，视图函数用于处理客户端发来的请求。
'''

# 配置文件myProject/settings.py中修改INSTALLED_APPS项添加创建的应用名称'myApp'将myApp应用安装到项目中：
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    #'myApp', #此处注释掉以避免重复报错
    'myApp.apps.MyappConfig',
]

# 模型文件修改储存为一次迁移：
$ python manage.py makemigrations myApp

# 执行SQL语句：
$ python manage.py sqlmigrate myApp 0001

# 创建新定义模型的数据表：
$ python manage.py migrate

# Django API （IPython）：
$ python manage.py shell
'''
In [1]: from myApp.models import Book
In [2]: Book.objects.all()   # 获取 Book 所有对象
Out[2]: <QuerySet []>
In [3]: from django.utils import timezone
In [4]: b = Book(name='Business', author='Tom', pub_house='First Press', pub_date=timezone.now())    #创建
In [5]: b.save() #保存
In [6]: b.id
Out[6]: 1
In [7]: b.name
Out[7]: 'Business'
In [8]: b.pub_date
Out[8]: datetime.datetime(2020, 4, 27, 7, 37, 59, 123686, tzinfo=<UTC>)
In [9]: exit()
'''

# 启动项目：
$ python manage.py runserver localhost:8080
