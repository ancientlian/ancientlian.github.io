---
title: 图书小项目理解Django（二）
date: 2018-12-18 22:36:53
tags: 
    - django 
    - python
categories: django
---
接上文：

## 信息展示页

### 设计url

在views里面：

```python
from django.shortcuts import render 
from booktest.models import BookInfo

def index(request):
    '''显示图书信息'''
    # 1. 查询出所有的图书信息
    books = BookInfo.objects.all()
    # 2. 设置模板，使用模板
    return render(request , 'booktest/index.html' , {'books':books})
```
<!--more-->
### 模板的创建

新建空directory名为tempates

在setting里的**TEMPLATES**处配置设定模板：`'DIRS': [os.path.join(BASE_DIR,'templates')],#设置模板目录`

新建app同名文件夹，内建index.html

### 配置urls

在文件bookM内配置urls为：

```python
urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'^' , include('booktest.urls')),   #包含boottest的urls文件
]
```

在app文件（也就是booktest）内新建urls.py，其内容为：

```python
from django.conf.urls import url
from booktest import views

urlpatterns = [
    url(r'^index$' , views.index),  #图书信息页面
    url(r'^create$' , views.create),#图书新增
    url(r'^delete(\d+)$',views.delete)#图书删除
    #   （\d+）的含义：让django进行地址匹配的时候能作为参数传递给views视图
]

```

### 设计html

编写index.html文件，内容为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书信息</title>
</head>
<body>
<a href="/create">新增一本图书</a>
{#如果create不加前面的/,而url里面刚好又在后面加了/,例如：r'^index/$'，#}
{#则会导致a标签访问的是http://127.0.0.1:8000/index/create，#}
{#而不是我们想要的http://127.0.0.1:8000/create，这样就会产生页面不存在的访问错误#}
<ul>
    {% for book in books %}
        <li>{{ book.btitle }}----<a href="/delete{{ book.id }}">删除</a></li>
    {% endfor %}
</ul>
</body>
</html>
```

## 业务逻辑

在views里编写业务逻辑

### 新增一本书：

```python
def create(request):
    '''新增加一本图书'''
    # 1. 创建Bookinfo对象
    b = BookInfo()
    b.btitle = '流星蝴蝶剑'
    b.bpub_date = date(1990,1,1)
    # 2. 保存进数据库
    b.save()
    # 3. 返回应答，让浏览器返回首页，也就是再访问/index,重定向
    #页面重定向：服务器不返回页面，而是告诉浏览器再去请求别的url地址
    #return HttpResponse('添加成功okokokok!!')  #这里不是给浏览器返回一个内容
    return  HttpResponseRedirect('/index')
```

### 删除一本书：

```python
def delete(request , bid):
    '''删除点击的图书'''
    # 1. 通过bid获取图书的对象
    book = BookInfo.objects.get(id = bid)
    # 2. 删除
    book.delete()
    # 3. 重定向，让浏览器重新访问/index
    #return HttpResponseRedirect('/index')
    # 上述函数可以简写为
    return redirect('/index')
```


附views页全代码及效果展示
