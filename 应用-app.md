# 应用-app

```
应用在Django项目中是一个独立的业务模块,可以包含自己的路由,视图,模板,模型
```

## 创建应用app

```python
用manage.py 中的子命令 startapp 创建应用文件夹
- python manage.py startapp 应用名
在settings.py 的 INSTALLED_APPS 列表中配置安装此应用
	# file : settings.py 
    INSTALLED_APPS = [
        ... ...,
        '自定义应用名称'
    ]
```

## 应用的结构组成

1. `migrations` 文件夹
   - 保存数据迁移的中间文件
2. `__init__.py`
   - 应用子包的初始化文件
3. `admin.py`
   - 应用的后台管理配置文件
4. `apps.py`
   - 应用的属性配置文件
5. `models.py`
   - 与数据库相关的模型映射类文件
6. `tests.py`
   - 应用的单元测试文件
7. `views.py`
   - 定义视图处理函数的文件

## 应用的分布式路由

```python
   Django中，基础路由配置文件(urls.py)可以不处理用户具体路由，基础路由配置文件的可以做请求的分发(分布式请求处理)。具体的请求可以由各自的应用来进行处理
```

### include函数

```python
作用: 
    用于分发将当前路由转到各个应用的路由配置文件的 urlpatterns 进行分布式处理

函数格式
	include('app名字.url模块名')
```

## 注意：

```python
1，先创建应用 再注册应用； 否则执行 python3 manage.py startapp 应用名时 会抛出 no module named 应用名 错误

2，一定要创建完应用后 即刻注册 【settings.py 里 添加自定义应用】

3，app下可以创建应用内部的templates文件夹，该文件夹可供开发人员存储当前应用下的html ;  如果settings.py中 TEMPLATES 配置了 DIRS属性【即配置了外部的html集中存储文件夹】, 则优先查找外部的存储文件夹，其次再查找应用内部的templates~

4，如果出现 TemplatesDoesNotExisted  则尝试重启进程 触发加载配置

5，当每个应用下都有templates时，请注意
	有可能当前应用加载了 其他应用中的 同名html
    
    解决方案： 
	1,app下的templates下创建同应用名的子文件夹，将该应用所有html转至该文件夹； ex: mysite3/music/templates/music/index.html

	2, music视图函数中， renturn render(request, 'music/index.html')
```

