---
title: Django配置mysql
date: 2018-12-19 22:36:53
tags: 
    - django 
    - python
    - mysql
categories: django
---
首先装好mysql数据库，django框架里面默认用的是sqlite的数据库

##  配置

在项目文件夹的settings.py中配置：

```python
DATABASES = {
    'default': {
        # 'ENGINE': 'django.db.backends.sqlite3',
        # 'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        'ENGINE': 'django.db.backends.mysql',   # 数据库引擎
        'NAME': 'tablename',         # 你要存储数据的库名，事先要创建之
        'USER': 'root',         # 数据库用户名
        'PASSWORD': 'mysql',     # 密码
        #'HOST': 'localhost',    # 主机
        'HOST':'127.0.0.1', # 制定mysql数据库所在电脑ip，如果是本机直接写‘localhost’
        'PORT': '3306',         # 数据库使用的端口
    }
}
```
<!--more-->
## 数据库的迁移

仅仅在settings里面配置好数据库的端口连接是还不够的，我们还需在几个步骤：

首先：在python的虚拟环境中需要**安装pymysql**，命令为`pip install pymysql`
（注：python2使用`pip install mysql-python` 来安装mysql-python

然后在项目文件夹下的-init-.py里面导入添加

```python
import pymysql
pymysql.install_as_MySQLdb()
```

之后就能在终端中进行数据库的迁移了：

```cmd
python manage.py makemigrations
python manage.py migrate
```

这样就简单完成了mysql数据库的配置了。