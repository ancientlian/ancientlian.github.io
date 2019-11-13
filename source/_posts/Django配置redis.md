---
title: Django配置redis
date: 2018-12-19 22:36:53
tags: 
    - django 
    - python
    - redis
categories: django
---
在django 中配置redis的连接

## linux安装

```
$ pip install django-redis
```

安装完成显示：`Successfully installed django-redis-4.10.0 redis-3.0.1`

## settings配置

settings里面的配置为：
<!--more-->
```python
# redis配置
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/5',
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        },
    },
    'session':{
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/6',
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        },
    },
}
```

这里注意的是我们get_redis_connection的方法默认使用的是default里面定义的数据库

我们看get_redis_connection的方法定义就明白了：

```python
def get_redis_connection(alias='default', write=True):
	"""
    Helper used for obtaining a raw redis client.
    """

    from django.core.cache import caches

    cache = caches[alias]

    if not hasattr(cache, "client"):
        raise NotImplementedError("This backend does not support this feature")

    if not hasattr(cache.client, "get_client"):
        raise NotImplementedError("This backend does not support this feature")

    return cache.client.get_client(write)

```

