---
title: Django中的Meta
date: 2018-12-19 22:36:53
tags: 
    - django 
    - python
categories: django
---
在Django模型类中经常会嵌套一个内部类，他的名字为Meta，它一般用于定义一些Django模型类的行为特性。也就是通过一个内嵌类 "class Meta" 给你的 model 定义元数据。

## 定义方式为：

```python
class Foo(models.Model): 
    example = models.CharField(maxlength=10)

    class Meta: 
        # 各种行为模式 #
```
<!--more-->
## 属性

### abstract

这个属性是定义当前的模型是不是一个**抽象类**。所谓抽象类是不会对应数据库表的。一般我们用它来归纳一些公共属性字段，然后继承它的子类可以继承这些字段。

Options.abstract
如果 `abstract = True` 这个model就是一个抽象类

### app_label

这个属性用于当你的模型不在默认的应用程序包下的models.py文件中，需要重新指定你这个模型所属应用程序。

Options.app_label
如果一个model定义在默认的models.py，例如如果你的app的models在另一个myapp.models子模块下，你必须定义app_label让Django知道它属于哪一个app
`app_label = 'myapp'`

### db_table

db_table是**指定自定义数据库表名**。Django有一套默认的按照一定规则生成数据模型对应的数据库表。
Options.db_table
定义该model在数据库中的表名称：
　　`db_table = 'Students'`
默认的命名方式为app_label + '_' + module_name 作为表名

### db_teblespace

Options.db_teblespace
定义这个model所使用的数据库表空间比如Oracle。你可以通过db_tablespace来指定这个模型对应的数据库表放在哪个数据库表空间。在项目的settings中定义。

### get_latest_by

Options.get_latest_by
在model中指定Manager上的lastest（）管理方法按照DateField或者DateTimeField类型的字段进行选取，默认使用指定字段来排序

### managed

Options.managed
默认值为True，这意味着Django可以使用syncdb和reset等命令来创建或移除对应的数据库。默认值为True,如果你不希望这么做，可以把manage的值设置为False

### order_with_respect_to

这个属性一般用于多对多的关系中，它指向一个关联对象，就是说关联对象找到这个对象后它是经过排序的。指定这个属性后你会得到一个get_xxx_order()和set_xxx_order()的方法，通过它们你可以设置或者回去排序的对象

### ordering

这个属性是告诉Django模型对象返回的记录结果集是按照哪个字段**排序**的。这是一个字符串的元组或列表，没有一个字符串都是一个字段和用一个可选的表明降序的'-'构成。当字段名前面没有'-'时，将**默认使用升序**排列。使用'?'将会随机排列

- ordering=['order_date'] 	# 按订单升序排列
- ordering=['-order_date']     # 按订单降序排列，-表示降序
- ordering=['?order_date']     # 随机排序，？表示随机
- ordering=['-pub_date','author']     # 以pub_date为降序，在以author升序排列

### permissions

permissions主要是为了在Django Admin管理模块下使用的，如果你设置了这个属性可以让指定的方法权限描述更清晰可读。Django自动为每个设置了admin的对象创建添加，删除和修改的权限。
`permissions = (('can_deliver_pizzas','Can deliver pizzas'))`

### proxy

这是为了实现**代理模型**使用的，如果proxy = True,表示model是其父的代理 model 

### unique_together

unique_together这个属性用于：当你需要通过两个字段保持唯一性时使用。比如假设你希望，一个Person的FirstName和LastName两者的组合必须是唯一的，那么可以这样设置：
`unique_together = (("first_name", "last_name"),)`
一个ManyToManyField不能包含在unique_together中。如果你需要验证关联到ManyToManyField字段的唯一验证，尝试使用signal(信号)或者明确指定through属性。

### verbose_name

verbose_name的用于给你的模型类起一个更可读的名字，一般定义为中文，例如：
`verbose_name = "学校"`

### verbose_name_plural

这个属性是指定模型的复数形式，比如：
`verbose_name_plural = "学校"`
如果不指定Django会自动在模型名称后加一个’s’