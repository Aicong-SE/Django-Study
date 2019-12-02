# mysql数据库

[toc]

## Django下配置使用 mysql 数据库

安装pymysql包

```python
$ sudo pip3 install pymysql
```

创建数据库

```python
$ create database mywebdb default charset utf8;
```

数据库配置

```python
# file: settings.py
DATABASES = {
    'default' : {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mywebdb',  # 数据库名称,需要自己定义
        'USER': 'root',
        'PASSWORD': '123456',  # 管理员密码
        'HOST': '127.0.0.1',
        'PORT': 3306,
    }
}
```

配置参数

```python
'ENGING' 指定数据库的后端引擎
	'django.db.backends.mysql'
    'django.db.backends.sqlite3'
    'django.db.backends.oracle'
    'django.db.backends.postgresql'
'NAME' 指定要连接的数据库的名称
'USER' 指定登录到数据库的用户名
'PASSWORD' 接数据库时使用的密码。
'HOST' 连接数据库时使用哪个主机。
'PORT' 连接数据库时使用的端口。
```

添加mysql 支持

```python
#file:  __init__.py
import pymysql
pymysql.install_as_MySQLdb()
```

#### 模型（Models）

```python
- 模型是一个Python类，它是由django.db.models.Model派生出的子类。
- 一个模型类代表数据库中的一张数据表
- 模型类中每一个类属性都代表数据表中的一个字段。
- 模型是数据交互的接口，是表示和操作数据库的方法和方式
```

## Django 的 ORM框架

```python
ORM（Object Relational Mapping）即对象关系映射，它是一种程序技术，它允许你使用类和对象对数据库进行操作,从而避免通过SQL语句操作数据库
```

###### ORM框架的作用

```python
1. 建立模型类和表之间的对应关系，允许我们通过面向对象的方式来操作数据库。
2. 根据设计的模型类生成数据库中的表格。
3. 通过简单的配置就可以进行数据库的切换。
```

###### ORM 好处

```python
1. 只需要面向对象编程, 不需要面向数据库编写代码.
   - 对数据库的操作都转化成对类属性和方法的操作.
   - 不用编写各种数据库的sql语句.
2. 实现了数据模型与数据库的解耦, 屏蔽了不同数据库操作上的差异.
   - 不在关注用的是mysql、oracle...等数据库的内部细节.
   - 通过简单的配置就可以轻松更换数据库, 而不需要修改代码.
```

###### ORM 缺点

```python
1. 相比较直接使用SQL语句操作数据库,有性能损失.
2. 根据对象的操作转换成SQL语句,根据查询的结果转化成对象, 在映射过程中有性能损失.
```

###### 模型示例：

- 此示例为添加一个 bookstore_book 数据表来存放图书馆中书目信息

- 添加一个 bookstore 的 app

  ```shell
  $ python3 manage.py startapp bookstore
  ```

- 添加模型类并注册app

  ```python
  # file : bookstore/models.py
  from django.db import models
  
  class Book(models.Model):
      title = models.CharField("书名", max_length=50, default='')
      price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
  ```

- 注册app

  ```python
  # file : setting.py
  INSTALLED_APPS = [
      ...
      'bookstore',
  ]
  ```

###### 数据库的迁移

```
迁移是Django同步您对模型所做更改（添加字段，删除模型等） 到您的数据库模式的方式
```

1 生成或更新迁移文件

```python
- 将每个应用下的models.py文件生成一个中间文件,并保存在migrations文件夹中
$ python3 manage.py makemigrations
```

2 执行迁移脚本程序

```python
- 执行迁移程序实现迁移。将每个应用下的migrations目录中的中间文件同步回数据库
$ python3 manage.py migrate
```

注：每次修改完模型类再对服务程序运行之前都需要做以上两步迁移操作。

## 模型类Models

```python
# 模型类需继承自`django.db.models.Model`
from django.db import models

class 模型类名(models.Model):
    字段名 = models.字段类型(字段选项)
    
```

#### 字段类型

1. BooleanField()

   - 数据库类型:tinyint(1)
   - 编程语言中:使用True或False来表示值
   - 在数据库中:使用1或0来表示具体的值

2. CharField()

   - 数据库类型:varchar
   - 注意:
     - 必须要指定max_length参数值

3. DateField()

   - 数据库类型:date
   - 作用:表示日期
   - 编程语言中:使用字符串来表示具体值
   - 参数:
     - DateField.auto_now: 每次保存对象时，自动设置该字段为当前时间(取值:True/False)。
     - DateField.auto_now_add: 当对象第一次被创建时自动设置当前时间(取值:True/False)。
     - DateField.default: 设置当前时间(取值:字符串格式时间如: '2019-6-1')。
     - 以上三个参数只能多选一

4. DateTimeField()

   - 数据库类型:datetime(6)
   - 作用:表示日期和时间
   - models.DateTimeField(auto_now_add=True,  )
   - default='2019-10-1 18:15:20'

5. DecimalField()

   - 数据库类型:decimal(x,y)

   - 编程语言中:使用小数表示该列的值

   - 在数据库中:使用小数

   - 参数:

     - DecimalField.max_digits: 位数总数，包括小数点后的位数。 该值必须大于等于decimal_places.
     - DecimalField.decimal_places: 小数点后的数字数量

   - 示例:

     ```python
     money=models.DecimalField(
         max_digits=7,
         decimal_places=2,
         default=0.0
     )
     ```

6. FloatField()

   - 数据库类型:double
   - 编程语言中和数据库中都使用小数表示值

7. EmailField()  

   - 数据库类型:varchar
   - 编程语言和数据库中使用字符串

8. IntegerField()

   - 数据库类型:int
   - 编程语言和数据库中使用整数

9. URLField()

   - 数据库类型:varchar(200)
   - 编程语言和数据库中使用字符串

10. ImageField()

    - 数据库类型:varchar(100)

    - 作用:在数据库中为了保存图片的路径

    - 编程语言和数据库中使用字符串

    - 示例:

      ```
      image=models.ImageField(
          upload_to="static/images"
      )
      ```

    - upload_to:指定图片的上传路径
      在后台上传时会自动的将文件保存在指定的目录下

11. TextField()

     - 数据库类型:longtext
     - 作用:表示不定长的字符数据

- 参考文档 <https://docs.djangoproject.com/en/1.11/ref/models/fields/#field-types>

#### 字段选项FIELD_OPTIONS

- 字段选项, 指定创建的列的额外的信息

- 允许出现多个字段选项,多个选项之间使用,隔开

1. primary_key
   - 如果设置为True,表示该列为主键,如果指定一个字段为主键，则此数库表不会创建id字段
2. blank
   - 设置为True时，字段可以为空。设置为False时，字段是必须填写的。
3. null
   - 如果设置为True,表示该列值允许为空。
   - 默认为False,如果此选项为False建议加入default选项来设置默认值
4. default
   - 设置所在列的默认值,如果字段选项null=False建议添加此项
5. db_index
   - 如果设置为True,表示为该列增加索引
6. unique
   - 如果设置为True,表示该字段在数据库中的值必须是唯一(不能重复出现的)
7. db_column
   - 指定列的名称,如果不指定的话则采用属性名作为列名
8. verbose_name
   - 设置此字段在admin界面上的显示名称。

- 示例:

  ```python
  # 创建一个属性,表示用户名称,长度30个字符,必须是唯一的,不能为空,添加索引
  name = models.CharField(max_length=30, unique=True, null=False, db_index=True)
  ```

- 文档参见:
  - <https://docs.djangoproject.com/en/1.11/ref/models/fields/#field-options>

## 数据库的基本操作

```python
- 数据库的基本操作包括增删改查操作，即(CRUD操作)
- CRUD是指在做计算处理时的增加(Create)、读取查询(Read)、更新(Update)和删除(Delete)
```

## 管理器对象

```python
	每个继承自 models.Model 的模型类，都会有一个 objects 对象被同样继承下来。这个对象叫管理器对象
    数据库的增删改查可以通过模型的管理器实现
```

## 创建数据对象

- Django 使用一种直观的方式把数据库表中的数据表示成Python 对象
- 创建数据中每一条记录就是创建一个数据对象

```python
1.MyModel.objects.create(属性1=值1, 属性2=值1,...)
- 成功: 返回创建好的实体对象
- 失败: 抛出异常
    
2.创建 MyModel 实例对象,并调用 save() 进行保存
    obj = MyModel(属性=值,属性=值)
    obj.属性=值
    obj.save()
    无返回值,保存成功后,obj会被重新赋值
```

## DJango shell 的使用

```python
- 在Django提供了一个交互式的操作项目叫 `Django Shell` 它能够在交互模式用项目工程的代码执行相应的操作
- 利用 Django Shell 可以代替编写View的代码来进行直接操作
- 在Django Shell 下只能进行简单的操作，不能运行远程调式
- 启动方式:
    $ python3 manage.py shell
```

## 查询数据

- 数据库的查询需要使用管理器对象进行 
- 通过 MyModel.objects 管理器方法调用查询接口

| 方法          | 作用                                                         | 返回值格式                              |
| ------------- | ------------------------------------------------------------ | --------------------------------------- |
| all()         | 查询MyModel实体中所有的数据                                  | QuerySet                                |
| values()      | 查询部分列的数据并返回                                       | QuerySet 内部：{'列1': 值1, '列2': 值2} |
| values_list() | 返回元组形式的查询结果                                       | QuerySet 内部：()                       |
| order_by()    | 与all()方法不同，它会用SQL 语句的ORDER BY 子句对查询结果进行根据某个字段选择性的进行排序[默认是按照升序排序,降序排序则需要在列前增加'-'表示] |                                         |
| filter(条件)  | 根据条件查询数据                                             | QuerySet                                |
| exclude(条件) | 返回不包含此 `条件` 的 全部的数据集                          |                                         |
| get(条件)     | 返回满足条件的唯一一条数据                                   | MyModel 对象                            |

### 字段查找

```python
- 字段查询是指如何指定SQL语句中 WHERE 子句的内容。
- 字段查询需要通过QuerySet的filter(), exclude() and get()的关键字参数指定。
- 非等值条件的构建,需要使用字段查询
    # 查询作者中年龄大于30
    Author.objects.filter(age__gt=30)
    # 对应
    # SELECT .... WHERE AGE > 30;
```

### 查询谓词

- 每一个查询谓词是一个独立的查询功能

1. `__exact` : 等值匹配

   ```python
   Author.objects.filter(id__exact=1)
   Author.objects.filter(id=1)
   # 等同于select * from author where id = 1
   ```

2. `__contains` : 包含指定值

   ```python
   Author.objects.filter(name__contains='w')
   # 等同于 select * from author where name like '%w%'
   ```

3. `__startswith` : 以 w 开始   like w%

4. `__endswith` : 以 w 结束     like  %w

5. `__gt` : 大于指定值

   ```python
   Author.objects.filer(age__gt=50)
   # 等同于 select * from author where age > 50
   ```

6. `__gte` : 大于等于

7. `__lt` : 小于

8. `__lte` : 小于等于

9. `__in` : 查找数据是否在指定范围内

   - 示例

   ```python
   Author.objects.filter(country__in=['中国','日本','韩国'])
   # 等同于 select * from author where country in ('中国','日本','韩国')
   ```

10. `__range`: 查找数据是否在指定的区间范围内

    ```python
    # 查找年龄在某一区间内的所有作者
    Author.objects.filter(age__range=(35,50))
    # 等同于 SELECT ... WHERE Author BETWEEN 35 and 50;
    ```

11. 详细内容参见: <https://docs.djangoproject.com/en/1.11/ref/models/querysets/#field-lookups>

- 示例

  ```python
  MyModel.objects.filter(id__gt=4)
  # 等同于 SELECT ... WHERE id > 4;
  ```


### 修改数据记录

```python
1. 修改单个实体的某些字段值:
    from bookstore import models
    abook = models.Book.objects.get(id=10)
    abook.market_price = "10.5"
    abook.save()
    
2. 通过 QuerySet 批量修改
-直接调用QuerySet的update(属性=值) 实现批量修改
	# 将 id大于3的所有图书价格定为0元
    books = Book.objects.filter(id__gt=3)
    books.update(price=0)
    # 将所有书的零售价定为100元
    books = Book.objects.all()
    books.update(market_price=100)
```

### 删除记录

```python
- 删除记录是指删除数据库中的一条或多条记录
- 删除单个MyModel对象或删除一个查询结果集(QuerySet)中的全部对象都是调用 delete()方法

    auths = Author.objects.filter('条件')
    auths.delete()
```

### 聚合查询

```
聚合查询是指对一个数据表中的一个字段的数据进行部分或全部进行统计查询
```

聚合函数：

- 定义模块：django.db.models
- 用法：from django.db.models import *
- 聚合函数：Sun, Avg, Count, Max, Min

1. 不带分组聚合

```python
MyModel.objects.aggregate(结果变量名=聚合函数('列'))
返回结果：{"结果变量名": 值}

result = MyModel.objects.aggregate(结果变量名=Avg('列'))
print(result['结果变量名'])
```

2. 分组聚合

```python
QuerySet.annotate(结果变量名=聚合函数('列'))
1.MyModel.objects.value('列1', '列2')
2.QuerySet.annotate(名=聚合函数('列'))
-返回 QuerySet 结果集,内部存储结果的字典

a = MyModel.objects.values('列1')
result = a.annotate(名=聚合函数('列1')
```







