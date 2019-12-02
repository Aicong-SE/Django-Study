# 项目结构

## 创建

```
$ django-admin startproject 项目名称
#运行
    $ python3 manage.py runserver
    # 或
    $ python3 manage.py runserver 127.0.0.1:5000  
    # 指定只能本机使用127.0.0.1的5000端口访问本机
```

### 项目目录结构

```
manage.py
此文件是项目管理的主程序,在开发阶段用于管理整个项目的开发运行的调式
```

#### 项目包文件夹

```
项目包的主文件夹(默认与项目名称一致)
__init__.py
    包初始化文件，当此项目包被导入(import)时此文件会自动运行
wsgi.py
    - WSGI 即 Web Server Gateway Interface
    - WEB服务网关接口的配置文件，仅部署项目时使用
urls.py
   - 项目的基础路由配置文件，所有的动态路径必须先走该文件进行匹配
settings.py
   - Django项目的配置文件, 此配置文件中的一些全局变量将为Django框架的运行传递一些参数
   - settings.py 配置文件,启动服务时自动调用，
   - 此配置文件中也可以定义一些自定义的变量用于作用全局作用域的数据传递
```

### settings.py 文件介绍

https://docs.djangoproject.com/en/1.11/ref/settings/

```
BASE_DIR 绑定当前项目的绝对路径
DEBUG True(调试模式)False(生产模式)
ALLOWED_HOSTS
    1. [] 空列表,表示只有`127.0.0.1`, `localhost`能访问本项目
    2. ['*']，表示任何网络地址都能访问到当前项目
    3. ['192.168.1.3', '192.168.3.3'] 表示只有当前两个主机能访问当前项目
INSTALLED_APPS 指定当前项目中安装的应用列表
MIDDLEWARE 用于注册中间件
TEMPLATES 用于指定模板的配置信息
DATABASES 用于指定数据库的配置信息
LANGUAGE_CODE 用于指定语言配置 (中文 : "zh-Hans" | 英文 : "en-us")
TIME_ZONE 用于指定当前服务器端时区(世界标准时间: "UTC" | 中国时区 : "Asia/Shanghai")
ROOT_URLCONF 用于配置根级 url 配置 'mysite1.urls'
```

### URL介绍

```
url 即统一资源定位符 Uniform Resource Locator
作用:用来表示互联网上某个资源的地址。
说明:互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。
URL的一般语法格式为：protocol :// hostname[:port] / path [?query][#fragment]
    protocol（协议）
    hostname（主机名）
    port（端口号）
    path（路由地址）
    query(查询)
    fragment（信息片断）
```

### 视图函数(view)

```
   视图函数是用于接收一个浏览器请求并通过HttpResponse对象返回数据的函数。此函数可以接收浏览器请求并根据业务逻辑返回相应的内容给浏览器
```

语法：

```
def xxx_view(request[, 其它参数...]):
    
    return HttpResponse对象
```

参数:

```
request用于绑定HttpRequest对象，通过此对象可以获取浏览器的参数和数据
```

### 路由配置

```
settings.py 中的`ROOT_URLCONF` 指定了主路由配置列表urlpatterns的文件位置
```

urls.py 主路由配置文件

```
# file : <项目名>/urls.py
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    ...  # 此处配置主路由
]
```

> urlpatterns 是一个路由-视图函数映射关系的列表,此列表的映射关系由url函数来确定

#### url() 函数

```
用于描述路由与视图函数的对应关系
from django.conf.urls import url
url(regex, views, name=None)
参数：
    regex: 字符串类型，匹配的请求路径，允许是正则表达式
    views: 指定路径所对应的视图处理函数的名称
    name: 为地址起别名，在模板中地址反向解析时使用
```

#### 带有分组的路由和视图函数

```
   在视图函数内，可以用正则表达式分组 `()` 提取参数后用函数位置传参传递给视图函数
    url(r'^http://127.0.0.1:8000/(\d+)$')
   在视图函数内，可以用正则表达式分组 `(?P<name>pattern)` 提取参数后用函数关键字传参传递给视图函数
    url(r'^http://127.0.0.1:8000/(?<num>\d+)')
```