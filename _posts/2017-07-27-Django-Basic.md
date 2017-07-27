---
layout: post
title: "【Python】Django入门"
date: 2017-07-27
tags: [Python, Django]
categories: [Python]
bgp: "bg_text_15"
---

## Django 概述

**来自百度百科**  

`Django`是一个`开放源代码`的`Web`应用框架，由`Python`写成。采用了`MVC`的框架模式，即模型M,视图V和控制器C. 它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的,即是CMS（内容管理系统）软件.并于2005年7月在`BSD`许可证下发布。这套框架是以比利时的吉普赛爵士吉他手Django Reinhardt来命名的.  

> 设计哲学

Django的主要目的是`简便`、`快速`的开发数据库驱动的网站。它强调`代码复用`,多个组件可以很方便的以`插件`形式服务于整个框架，Django有许多功能强大的第三方插件，你甚至可以很方便的开发出自己的工具包。这使得Django具有很强的`可扩展性`。它还强调快速开发和DRY(Do Not Repeat Yourself)原则。
Django基于`MVC`的设计十分优美:

* 对象关系映射 (ORM,object-relational mapping)：以Python类形式定义你的数据模型，ORM将模型与关系数据库连接起来，你将得到一个非常容易使用的数据库API，同时你也可以在Django中使用原始的SQL语句。
* URL 分派：使用正则表达式匹配URL，你可以设计任意的URL，没有框架的特定限定。像你喜欢的一样灵活
* 模版系统：使用Django强大而可扩展的模板语言，可以分隔设计、内容和Python代码。并且具有可继承性。
* 表单处理：你可以方便的生成各种表单模型，实现表单的有效性检验。可以方便的从你定义的模型实例生成相应的表单。
* Cache系统：可以挂在内存缓冲或其它的框架实现超级缓冲————实现你所需要的粒度。
* 会话(session): 用户登录与权限检查，快速开发用户会话功能。
* 国际化：内置国际化系统，方便开发出多种语言的网站。
* 自动化的管理界面：不需要你花大量的工作来创建人员管理和更新内容。Django自带一个ADMIN site,类似于内容管理系统.

> 文件概述

**urls.py**

链接入口，关联到对应的 views.py 中的一个函数（或者 generic 类），访问的链接就对应一个函数。

**views.py**

处理用户发出的请求，从 urls.py 中对应而来，通过渲染 templates 中的网页可以为用户显示页面内容，比如登录后的用户名，用户请求的数据，通过其输出到页面。

**models.py**

与数据库操作相关，存入或读取数据时使用。当不使用数据库的时候，也可以当做一般的类封装文件，存储各种类的定义。

**forms.py**

表单，用户在浏览器上输入提交，对数据的验证工作以及输入框的生成等工作，都依托于此。

**admin.py**

后台文件，可以用少量的代码就拥有一个强大的后台。

**settings.py**

Django 的设置、配置文件，比如 DEBUG 的开关，静态文件的位置等等。

**templates目录**

views.py 中的函数渲染 templates 中的 html 模板，得到动态内容的网页，可以用缓存来提高渲染速度。

## Hello World

**本机环境：macOS Sierra 10.12.5 python 2.7+django 1.11.3**  

> 创建项目

```python
python django-admin.py startproject InterestCircle
```

> 创建App

```python
python manage.py startapp Start
```

> 配置App

```python
vim InterestCircle/InterestCircle/settings.py

# 添加App
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'Start',
)
```

> 修改views.py

```python
vim InterestCircle/Start/views.py

# -*- coding: utf-8 -*-
from django.shortcuts import render
from django.http import HttpResponse


def index(request):
    return HttpResponse(u"Hello World")

# import HttpResponse, 它是用来向网页返回内容的，就像 Python 中的 print() 函数一样，只不过 HttpResponse 是把内容显示到网页上.

# 最后定义了一个 index() 函数, 第一个参数必须是 request, request 包含了 get 或 post 方式传递而来的参数。我们可以对这些参数做出定制处理，然后向用户层返回待展示的数据。
```

> 修改urls.py

```python
vim InterestCircle/InterestCircle/urls.py

from django.conf.urls import url
from django.contrib import admin
from Start import views as Start_views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', Start_views.index),
]
```

> 启动服务器

```python
python manage.py runserver 8080
```
