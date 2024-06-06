# Django

# 设计模式

Django无需数据库就可以使用，因为它提供了对象关系映射器，通过此技术，你可以使用 Python 代码来描述数据库结构。

# Django文件架构

```cpp
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

- **`manage.py`**: 一个让你用各种方式管理 Django 项目的命令行工具
- **`mysite/__init__.py`**：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包
- **`mysite/settings.py`**：Django 项目的配置文件
- **`mysite/urls.py`**：Django 项目的 URL 声明，就像你网站的“目录”
- **`mysite/asgi.py`**：作为你的项目的运行在 ASGI 兼容的Web服务器上的入口
- **`mysite/wsgi.py`**：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口

[详情请见](https://docs.djangoproject.com/zh-hans/5.0/intro/tutorial01/)

## 启动开发服务器

`py manage.py runserver`

### 更换端口

`py manage.py runserver 8080`

**向网络上的其它电脑展示你的成果时**：
`py manage.py runserver 0.0.0.0:8000`

# 应用 & 项目

应用：

- 是一个专门做某件事的网络应用程序

项目：

- 是一个网站使用的配置和应用的集合

## 创建一个应用

`py manage.py startapp polls`

创建完的应用目录如下：

```cpp
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

视图（view）：

- 是一个负责处理用户请求并返回相应响应的Python函数或类
- 视图可以读取请求数据、处理业务逻辑、访问数据库，并最终返回一个响应对象（例如HTML页面、JSON数据或简单的文本响应）

在编写完视图之后，需要一个 `URL` 去映射到它，这样就可以看见效果

如何映射：

1. 创建文件 **`polls/urls.py`**
2. 在 **`polls/urls.py` 中写下代码：**

```cpp
from django.urls import path

from . import views

urlpatterns = [
    path("", views.index, name="index"),
]
```

- `urlpatterns`：这是一个列表，其中包含了 URL 模式与视图函数的映射
- `path` 函数用于将一个 URL 模式与视图函数关联
- 第一个参数 `""` 表示空字符串，即根 URL
- 第二个参数 `views.index` 是将根 URL 映射到 `views.index` 视图函数
- 第三个参数 `name="index"` 是给这个 URL 模式一个命名，这样可以在 Django 的其他地方通过这个名字引用这个 URL
1. 在根 URLconf 文件中指定我们创建的 **`polls.urls`**

```cpp
urlpatterns = [
    path("polls/", include("polls.urls")),
    path("admin/", admin.site.urls),
]

```

- `path("polls/", ...)`：定义了一个 URL 模式，以 `"polls/"` 开头的所有 URL 都会匹配这个模式
- `include("polls.urls")`：`include` 函数包含了 `polls` 应用的 URLconf 模块。`polls.urls` 是指向 `polls` 应用中的 `urls.py` 文件
    - 当用户访问以 `"polls/"` 开头的 URL 时，Django 会将剩余的 URL 交给 `polls.urls` 模块处理

[更多关于path()的信息](https://docs.djangoproject.com/zh-hans/5.0/ref/urls/#django.urls.path)

# 数据库配置

- 默认数据库
    - SQLite
- 想使用其他数据库
    - 安装合适的 [database bindings](https://docs.djangoproject.com/zh-hans/5.0/topics/install/#database-installation)
    - 改变设置文件中 [**`DATABASES`**](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-DATABASES) **`'default'`** 项目中的一些键值：
        - [**`ENGINE`**](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-DATABASE-ENGINE) -- 可选值有：
            - **`'django.db.backends.sqlite3'`**，**`'django.db.backends.postgresql'`**，**`'django.db.backends.mysql'`**，或 **`'django.db.backends.oracle'`**
        - [**`NAME`**](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-NAME) -- 数据库的名称
    - 请确认在使用前已经创建了数据库确保该数据库用户中提供 **`mysite/settings.py`** 具有 "create database" 权限
    
    [详情见database文档](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-DATABASES)
    

**编辑 `mysite/settings.py` 文件前，先设置 [`TIME_ZONE`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-TIME_ZONE) 为你自己时区**
`py manage.py migrate` ：根据mysite/settings.py文件中的数据库配置和随应用提供的数据库迁移文件，**创建任何必要的数据库表**