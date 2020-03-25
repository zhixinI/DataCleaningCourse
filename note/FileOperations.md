# 文件操作

### 1.csv读写

```python
import pandas as pd
import numpy as np
#设置最大显示列数
pd.set_option('display.max_columns',20)

#设置最大显示行数
pd.set_option('display.max_rows',100)

#婴儿信息表;默认将第一行作为表头，一般用utf-8编码
baby = pd.read_csv('../data/sam_tianchi_mum_baby.csv',encoding='utf-8')

#订单数据；gbk中文编码
order = pd.read_csv('../data/meal_order_info.csv',encoding='gbk',dtype={'info_id':str,'emp_id':str})

#交易表;nrows=10读取前10行
baby_trade_history = pd.read_csv('../data/baby_trade_history.csv',nrows=10)
baby_trade_history
```

存数据

```python
#建议用utf-8或者中文gbk编码默认utf-8，index=False表示不写出索引行
baby_trade_history.to_csv('../data/bthc.csv',encoding='utf-8',index=False)
```

### 2.excel文件读写

```python
#订单数据;sheet_name=0,表示第几个工作薄
dfe2 = pd.read_excel('../data/meal_order_detail.xlsx',encoding='utf-8',sheet_name=0)
```

存数据

```python
#保存数据sheet_name='one',即工作薄名字
dfe2.to_excel('../data/dfe2.xlsx',sheet_name='one',index=False)
```

### 3.数据库文件读写

```python
import pymysql
from sqlalchemy import create_engine
```

- 按实际情况依次填写MySQL的用户名、密码、IP地址、端口、数据库名
- create_engine('mysql+pymysql://user:passward@IP:3306/test01')
    - root 用户名
    - passward --密码
    - IP : 服务区IP
    - 3306： 端口号
    - FileOperations :数据库名称
- 建库
    - mysql> create database FileOperations charset 'utf8';

建立连接

```python
coon = create_engine('mysql+pymysql://root:password@localhost:3306/数据库')
```

读取数据；选择数据库中表的名称

```python
sql = 'select * from chinaAdd'
df1 = pd.read_sql(sql,coon)
df1
```

```python
#函数
def querty(table):
    host = 'localhost'
    user = 'root'
    password = 'password'
    database = '数据库'
    port = 3306
    conn = create_engine("mysql+pymysql://{}:{}@{}:{}/{}".format(user, password, host, port, database))
    #sql语句，可以定制，实现灵活查询;table:选择数据库中表名称
    sql = 'select * from '+ table
    #使用pandas的read_sql函数直接将数据存dataframe中
    results = pd.read_sql(sql,coon)
    return results
```

```python
df2 = querty('chinaAdd')
df2
```

数据保存
- df.to_sql(name, con=engine, if_exists='replace/append/fail',index=False)
- name是表名
- con是连接
- if_exists：表如果存在怎么处理 -- append：追加 -- replace：删除原表，建立新表再添加 -- fail：什么都不干
- index=False：不插入索引index

```python
dfc = pd.read_csv('../data/baby_trade_history.csv')
try:
    dfc.to_sql('数据库名',con=coon,index=False,if_exists='replace')
except:
    print('error')
```

Python是否能将数据写入数据库，很多时候取决于数据库的权限





























































