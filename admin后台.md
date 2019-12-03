[toc]

# admin后台

```python
django 提供了比较完善的后台管理数据库的接口，可供开发过程中调用和测试使用
django 会搜集所有已注册的模型类，为这些模型类提拱数据管理界面，供开发者使用
```

### 使用步骤

1. 创建后台管理帐号:

   - 后台管理--创建管理员帐号

     - `$ python3 manage.py createsuperuser`            
     - 根据提示完成注册,参考如下:

     ```shell
     $ python3 manage.py createsuperuser
     Username (leave blank to use 'tarena'): tarena  # 此处输入用户名
     Email address: laowei@tedu.cn  # 此处输入邮箱
     Password: # 此处输入密码(密码要复杂些，否则会提示密码太简单)
     Password (again): # 再次输入重复密码
     Superuser created successfully.
     $ 
     ```

2. 用注册的帐号登陆后台管理界面

   - 后台管理的登录地址:
     - <http://127.0.0.1:8000/admin>

## 自定义后台管理数据表

- 添加自己定义模型类的后台管理数据表,需要用`admin.site.register(自定义模型类)` 方法进行注册

```python
1.在应用app中的admin.py中导入注册要管理的模型models类,
from . import models
2.调用 admin.site.register 方法进行注册,如:
from django.contrib import admin
admin.site.register(自定义模型类)

示例：
# file: bookstore/admin.py
from django.contrib import admin
# Register your models here.
from . import models
...
admin.site.register(models.Book)  # 将Book类注册为可管理页面
```

## 修改后台Models的展现形式

```python
在admin后台管理数据库中对自定义的数据记录都展示为 `XXXX object` 类型的记录，不便于阅读和判断,在用户自定义的模型类中可以重写 def __str__(self): 方法解决显示问题
    class Book(models.Model):
    ...
    def __str__(self):
        return "书名" + self.title
```

## 模型管理器类

作用:为后台管理界面添加便于操作的新功能。

说明:后台管理器类须继承自 `django.contrib.admin` 里的 `ModelAdmin` 类

```python
1.在 <应用app>/admin.py 里定义模型管理器类
class XXXX_Manager(admin.ModelAdmin):
    ......
2.注册管理器与模型类关联
from django.contrib import admin
from . import models
admin.site.register(models.YYYY, XXXX_Manager) # 注册models.YYYY 模型类与 管理器类 XXXX_Manager 关联

示例：
# file : bookstore/admin.py
from django.contrib import admin
from . import models

class BookAdmin(admin.ModelAdmin):
    list_display = ['id', 'title', 'price', 'market_price']

admin.site.register(models.Book, BookAdmin)
```

#### 模型管理器类ModelAdmin中实现的高级管理功能

1. list_display 去控制哪些字段会显示在Admin 的修改列表页面中。
2. list_display_links 可以控制list_display中的字段是否应该链接到对象的“更改”页面。
3. list_filter 设置激活Admin 修改列表页面右侧栏中的过滤器
4. search_fields 设置启用Admin 更改列表页面上的搜索框。 
5. list_editable 设置为模型上的字段名称列表，这将允许在更改列表页面上进行编辑。
6. 其它参见<https://docs.djangoproject.com/en/1.11/ref/contrib/admin/>

## 数据库表管理

1. 修改模型类字段的显示名字

   - 模型类各字段的第一个参数为 verbose_name,此字段显示的名字会在后台数据库管理页面显示

   - 通过 verbose_name 字段选项,修改显示名称示例如下：

     ```python
     title = models.CharField(
         max_length = 30,
         verbose_name='显示名称'
     )
     ```

2. 通过Meta内嵌类 定义模型类的属性及展现形式

   - 模型类可以通过定义内部类class Meta 来重新定义当前模型类和数据表的一些属性信息

   - 用法格式如下:

     ```python
     class Book(models.Model):
         title = CharField(....)
         class Meta:
             1. db_table = '数据表名'
                 - 该模型所用的数据表的名称。(设置完成后需要立马更新同步数据库)
                 python3 manage.py makemigrations
                 python3 manage.py migrate
                 
             2. verbose_name = '单数名'
                 - 给模型对象的一个易于理解的名称(单数),用于显示在/admin管理界面中
             3. verbose_name_plural = '复数名'
                 - 该对象复数形式的名称(复数),用于显示在/admin管理界面中
     ```

