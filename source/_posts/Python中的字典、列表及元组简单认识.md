---
title: Python中的字典、列表及元组简单认识
date: 2019-11-14 22:36:53
tags:  
    - python
categories: Python基础
---
本文主要简单介绍Python当中字典（Dictionary）、列表（List）以及元组（Tuple）的基础知识。
## 简介

在我们的python中有一些特有的内置数据类型，其中包括有dictionary（字典）、tuple（元组）、list（列表）。

## Dictionary

Python的内置数据类型之一，定义了**键值**之间一对一的关系。

### 定义

```python
d = {"test1":"test1","test2":111}
>>>d["test2"]	#引用方法
>>>111
```

每一个元素都是一个key-value对，整个元素集合用{}括起来。**不允许使用值获取key。**
dictionary的值可以**混用**，包括字符串、整数、对象、甚至是dictionary。但是key会严格一点。
<!--more-->
### 修改

我们可以对字典其中的数据进行修改。
```python
d["test2"] = "222"
d["test3"] = "test3"	#修改不存在的，实际会添加一个新的键值对
```
**Dictionary不能使用重复的key**，它其实会对原来的key进行重新赋值。在任何的时候都可以加入新的key-value对，这种语法同修改存在的值是一样的。
**Dictionary是无序的！**
**Dictionary是大小写敏感的！**
```python
d = {}
d["key"] = "value"
d["Key"] = "other value"
d["Key"] = "change value"	#修改
>>>d
>>>{'Key': 'change value', 'key': 'value'}
```

### 删除

我们使用简单的del来进行dictionary元素的删除，clear 从一个 dictionary 中清除所有元素。
```python
d = {111:"test1", "test2":"222", "test3":333}
del d[111]
>>>d
>>>{"test2":"222", "test3":333}
d.clean()
>>>d
>>>{}	# 没有元素的字典
```

## List

list是使用最频繁的数据类型，它更像java中的数组，或者说是ArrayList，可以保存任意对象，并且可以在增加新元素时动态扩展。

### 定义

```python
li = ["a", "b", "c", "z", "example"]
>>>li[0]
>>>'a'
>>>li[4]
>>>'example'
```
List是一个方括号包括起来的**有序**元素的集合。**从下标0开始进行计数**，第一个非空元素为li[0]，如果查找越界会报错`list index out of range`。

### 查找
```python
li = [1,2,3]
>>>li[-1]
>>>3
```
负数索引从list尾部向前计数，最后一个非空元素为list[-1]，如果查找越界会报错`list index out of range`。
#### 分片（slice）
```python
li = [1,2,3,4,5,6]
>>>li[1:3]
>>>[2,3]
>>>li[1:-1]
>>>[2,3,4,5]
>>>li[-3:-1]
>>>[4,5]
>>>li[-1:-3]
>>>[]
```
我们可以通过两个索引切片得到一个list子集，返回一个新的list，注意**切片包括第一个索引不包括第二个索引**。
list支持切片的简写

```python
li = [1,2,3,4,5]
>>>li[:3]
>>>[1,2,3]
>>>li[3:]
>>>[4,5]
>>>li[:]
>>>[1,2,3,4,5]
```
li[:3]等同于li[0:3]，li[3:]等同于li[3:5]。两边分片索引全部省略，这将包括所有的list元素。

### 增加

我们有三种常用的方法来给list增加元素，分别为append()、insert()、extend()。

```python
li = [1,2,3]
li.append(4)
>>>li
>>>[1,2,3,4]
li.insert(2,"a")
>>>li
>>>[1,2,'a',3,4]
li.extend([1,3])
>>>li
>>>[1,2,'a',3,4,1,3]
```

append()是向list末尾追加单个元素
insert()是元素插入到索引位置。如上所见，元素并不唯一，同时出现了两个1元素。
extend()用以连接list。

#### extend与append的区别
```python
li = [1,2,3]
li.extend([4,5])
>>>li
>>>[1,2,3,4,5]	#len = 5

li.append([4,5])
>>>li
>>>[1,2,3,[4,5]]	#len = 4
>>>li[-1]
>>>[4,5]
```
如上所见，**extend只接受一个list参数，且依次添加到原list**。
**append接受一个任何数据类型的参数，且仅仅追加一项到list尾部**。如上追加的为一个list，list可以包含任何数据类型，自然也包括list。
明确使用extend和append的场合，避免产生不想要的效果。

### 查找
```python
li = [1,2,3,2]
>>>li.index(2)
>>>1
>>>li.index(2,2)
>>>3
>>>"a" in li
>>>False

```
index查找一个值首次出现的索引值。当然我们也可以**指定第n个出现m的索引值**index(m,n)。
如果未找到，python会抛出一个异常`ValueError: list.index(x): x not in list`
我们可以**使用in测试一个值是否在list内**。存在返回True，否则False。

### 删除
```python
li = [1,2,1,3,1,4]
>>>li.remove(1)
>>>[2,1,3,1,4]
>>>li.pop()
>>>4
>>>li
>>>[2,1,3,1]

```
remove从列表中删除一个值的首次出现，并不会返回值。如果没有找到这个值，会抛出异常`ValueError: list.remove(x): x not in list`
pop首先会**删除list最后的一个值，并返回**。

### list的运算符
```python
li = [1,2,3]
li = li + ['a', 'b']
>>>li
>>>[1,2,3,'a','b']
li += ['new']
>>>li
>>>[1,2,3,'a','b','new']
li = [1,2]*3
>>>li
>>>[1,2,1,2,1,2]
```
list可以用+运算符连接，返回一个添加后的列表，等同于extend()。但extend修改存在的list，大型list，extend执行速度更快。

## Tuple

Tuple是**不可变**的list。
```python
>>>t = (1,3,2)
>>>t[0]
1
>>>t[-1]
2
>>>t[1:3]
(3,2)
```
tuple使用小括号进行定义，且按定义顺序进行排序。
tuple支持负数索引查找，且同样可以使用切片，返回一个新的tuple。
tuple不存在append、extend、remove、pop、index等方法。但是**支持in来查看元素是否存在**。
