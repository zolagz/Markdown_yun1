不同的数据库访问都要有不同的接口程序，Python DB API就是为了解决这一混乱现象而产生的Python DB API ：Python访问数据库的统一接口规范

[规范文档](https://www.python.org/dev/peps/pep-0249/)

有了Python DB API之后，可以使用统一接口对不通数据库的访问

Python DB API包含的内容 ：

数据库连接对象：connection
用于连接程序和数据库服务器

数据库交互对象：cursor
用于程序和数据库服务器之间传递sql语句

数据库异常类：exceptions
处理数据库运行中出现的故障

使用Python DB API访问数据库流程：

> 开始→创建connection（建立网络连接）→获取cursor（用来交互对象）→执行相关操作（执行查询，执行命令，获取数据，处理数据）→关闭cursor→关闭connection→结束

为什么要关闭connection呢？
因为connection是网络连接，如果不及时关闭，会对数据库的性能有影响。

数据库连接

数据库连接前我们先确认一下事项：
我们已创建数据库test，在test库里创建了表account，表account里有acctid和money两个字段。

在连接之前我们先明确一个知识：什么是PyMySQL？
PyMySQL 是在 Python3.x 版本中用于连接 MySQL 服务器的一个库，Python2中则使用mysqldb。
PyMySQL 遵循 Python 数据库 API v2.0 规范，并包含了 pure-Python MySQL 客户端库。
可以直接使用pip install PyMySQL安装模块

连接mysql数据库：

```

import pymysql

#打开数据库的连接，创建connection
db=pymysql.connect("localhost","root","root","test")

#使用cursor()方法创建一个游标对象（数据库交互对象）
cursor=db.cursor()

#使用execute()方法执行SQL查询
cursor.execute("SELECT VERSION()")

#使用fetchone()方法获取点条数据
data=cursor.fetchone()

print("Database version: %s " % data)

#关闭数据库连接
db.close()

```

###创建数据库表：

在连接存在的情况下，我们可以使用execute()方法来问数据库创建表，如下所示创建表users：

```

importpymysql

#打开数据库的连接，创建connection
db=pymysql.connect("localhost","root","root","test")

#使用cursor()方法创建一个游标对象（数据库交互对象）
cursor=db.cursor()

#使用execute()方法执行SQL，如果表存在则删除
cursor.execute("DROP TABLE IF EXISTS users")

sql='''create table users(
namevarchar(20) not null,
ageSMALLINT,
sexCHAR (1))'''

cursor.execute(sql)
#关闭数据库连接

db.close()

```

数据库插入操作：

```
#打开数据库的连接，创建connection
db=pymysql.connect("localhost","root","root","test")

#使用cursor()方法创建一个游标对象（数据库交互对象）
cursor=db.cursor()

#SQL插入语句
sql='''insert into users VALUES ('caizhenghua',30,0)'''

try:
#执行sql语句
    cursor.execute(sql)
    #提交到数据库
    db.commit()
except:
#如果发生错误则回滚
    db.rollback()

#关闭数据库连接
db.close()

```


数据库查询操作：

Python查询Mysql数据库使用fetchone()方法获取单条数据，

使用fetchall()方法获取多条数据。

fetchone():该方法获取下一个查询结果集。结果集是一个对象。
fetchall()接受全部的返回结果行。
rowcount()：这时一个只读属性，并返回执行execute()方法后影响的行数

小栗子：查询users表中年龄大于25的人员：

数据库更新操作：

```
import pymysql

#打开数据库的连接，创建connection
db=pymysql.connect("localhost","root","root","test")

#使用cursor()方法创建一个游标对象（数据库交互对象）
cursor=db.cursor()

#SQL插入语句
sql='''select*from users where age > '%d' '''% (25)
try:
    #执行sql语句
    cursor.execute(sql)
    #获取所有记录列表
    results=cursor.fetchall()
    for row in results:
        name=row[0]
        age =row[1]

    #打印结果
    print("name=%s,age=%s"% (name,age))
except:
    #发生错误是输出提示
    print("Error:unable to fecth data")

#关闭数据库连接
db.close()



```

更新数据库数据，将性别类全部改为 1。

```
import pymysql

#打开数据库的连接，创建connection
db=pymysql.connect("localhost","root","root","test")

#使用cursor()方法创建一个游标对象（数据库交互对象）
cursor=db.cursor()

#SQL插入语句
sql='''update users  set sex=1 '''
try:
    #执行sql语句
    cursor.execute(sql)
    #提交到数据库
    db.commit()
except:
    #发生错误时回滚
    db.rollback()

#关闭数据库连接
db.close()

```

删除操作：
删除数据库数据，将年龄小于25的全部删除。

```
importpymysql

#打开数据库的连接，创建connection
db=pymysql.connect("localhost","root","root","test")

#使用cursor()方法创建一个游标对象（数据库交互对象）
cursor=db.cursor()

#SQL插入语句
sql='''delete from users where age < '%d' '''% (25)
try:
    #执行sql语句
    cursor.execute(sql)
    #提交到数据库
    db.commit()
except:
    #发生错误时回滚
    db.rollback()
#关闭数据库连接
db.close()

```

执行事务：
事务机制可以确保数据的一致性。

事务应该具有4个属性：原子性，一致性，隔离性，持久性。这四个属性通常称为ACID特性。

原子性（atomicity）。一个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做。

一致性（consistency）。事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。

隔离性（isolation）。一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。

持久性（durability）。持续性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。

Python DB API 2.0 的事务提供了两个方法 commit 或 rollback。

栗子：

```
#SQL删除语句
sql='''delete from users where age < '%d' '''% (25)

try:
    #执行sql语句
    cursor.execute(sql)
    #提交到数据库
    db.commit()
except:
    #发生错误时回滚
    db.rollback()

```
对于支持事务的数据库， 在Python数据库编程中，当游标建立之时，就自动开始了一个隐形的数据库事务。

commit()方法游标的所有更新操作，rollback（）方法回滚当前游标的所有操作。每一个方法都开始了一个新的事务。

错误处理：
DB API中定义了一些数据库操作的错误及异常，下表列出了这些错误和异常:

异常描述

Warning当有严重警告时触发，例如插入数据是被截断等等。必须是 StandardError 的子类。

Error警告以外所有其他错误类。必须是 StandardError 的子类。

InterfaceError当有数据库接口模块本身的错误（而不是数据库的错误）发生时触发。 必须是Error的子类。

DatabaseError和数据库有关的错误发生时触发。 必须是Error的子类。

DataError当有数据处理时的错误发生时触发，例如：除零错误，数据超范围等等。 必须是DatabaseError的子类。

OperationalError指非用户控制的，而是操作数据库时发生的错误。例如：连接意外断开、 数据库名未找到、事务处理失败、内存分配错误等等操作数据库是发生的错误。 必须是DatabaseError的子类。

IntegrityError完整性相关的错误，例如外键检查失败等。必须是DatabaseError子类。

InternalError数据库的内部错误，例如游标（cursor）失效了、事务同步失败等等。 必须是DatabaseError子类。

ProgrammingError程序错误，例如数据表（table）没找到或已存在、SQL语句语法错误、 参数数量错误等等。必须是DatabaseError的子类。

NotSupportedError不支持错误，指使用了数据库不支持的函数或API等。例如在连接对象上 使用.rollback()函数，然而数据库并不支持事务或者事务已关闭。 必须是DatabaseError的子类。

作者：海贼之路飞
链接：http://www.jianshu.com/p/e73ef1efdc1d
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。