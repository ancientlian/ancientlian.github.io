---
title: 图书小项目理解Django（一）
date: 2018-12-18 21:36:53
tags: 
    - django 
    - python
categories: django
---
在Django项目中，我们会对扑面而来的众多框架内置生成文件望而生畏，这里通过一个简单的图书馆管理小例子来简单说明Django内置各部分的作用，便于快速理解Django

## 前期准备

进入虚拟环境（[如何进入虚拟环境]()）	

终端输入：

```
django-admin startproject bookM	#创建工程
cd bookM	#进入到项目文件
python manage.py startapp booktest	#创建app

```
<!--more-->
## 建立模型类

这样就已经基本创建好项目所用，我使用的IDE是Pycharm
打开后我们进入到booktest的models开始创建模型类：

```python
from django.db import models

# Create your models here.
class BookInfo(models.Model):
    '''图书模型类'''
    #图书名称
    btitle = models.CharField(max_length = 20)
    #出版日期
    bpub_date = models.DateField()
    #阅读量
    bread = models.IntegerField(default = 0)
    #评论量
    bcomment = models.IntegerField(default = 0)
    #删除标记
    isDelete = models.BooleanField(default=False)

class HeroInfo(models.Model):
    '''英雄人物模型类'''
    hname = models.CharField(max_length = 20)
    #性别
    hgender = models.BooleanField(default = False)
    #备注
    hcomment = models.CharField(max_length = 200)
    #关系属性
    hbook = models.ForeignKey('Bookinfo')
    # 删除标记
    isDelete = models.BooleanField(default=False)


```

现在类对应的表还没有，所以还需要迁移。

## 迁移

**第一步：**终端输入：`python manage.py makemigrations`

**第二步：**生成迁移文件之后输入`python manage.py migrate`，根据迁移文件生成mysql的表

你可以在终端中通过mysql简单查看是否输入进去导入成功

```
mysql> use bookM；
mysql> show tables；
--其中有--
+----------------------------+
| Tables_in_bookM            |
+----------------------------+
| auth_group                 |
| auth_group_permissions     |
| auth_permission            |
| auth_user                  |
| auth_user_groups           |
| auth_user_user_permissions |
| booktest_bookinfo          |
| booktest_heroinfo          |
| django_admin_log           |
| django_content_type        |
| django_migrations          |
| django_session             |
+----------------------------+
12 rows in set (0.00 sec)

mysql> desc booktest_bookinfo；
+-----------+-------------+------+-----+---------+----------------+
| Field     | Type        | Null | Key | Default | Extra          |
+-----------+-------------+------+-----+---------+----------------+
| id        | int(11)     | NO   | PRI | NULL    | auto_increment |
| btitle    | varchar(20) | NO   |     | NULL    |                |
| bpub_date | date        | NO   |     | NULL    |                |
| bread     | int(11)     | NO   |     | NULL    |                |
| bcomment  | int(11)     | NO   |     | NULL    |                |
| isDelete  | tinyint(1)  | NO   |     | NULL    |                |
+-----------+-------------+------+-----+---------+----------------+
6 rows in set (0.01 sec)

mysql> desc booktest_heroinfo;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| hname    | varchar(20)  | NO   |     | NULL    |                |
| hgender  | tinyint(1)   | NO   |     | NULL    |                |
| hcomment | varchar(200) | NO   |     | NULL    |                |
| isDelete | tinyint(1)   | NO   |     | NULL    |                |
| hbook_id | int(11)      | NO   | MUL | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)

---添加一点数据---
mysql>insert into booktest_bookinfo(btitle,bpub_date,bread,bcomment,isDelete) values
('射雕英雄传','1980-5-1',12,34,0)；
mysql>insert into booktest_heroinfo(hname,hgender,hbook_id,hcomment,isDelete) values
('郭靖',1,1,'降龙十八掌',0),
---

```

转下文