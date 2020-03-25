# 数据统计

#### 1.数据分组运算

- df.groupby('a')
- df.groupby(by=['a','b'])
- df[['a','b','c']].groupby('a')
- df[['a','b','c']].groupby(by=['a','b'])
- 使用单个分组变量
- 使用groupby方法

```python
import pandas as pd
import numpy as np
df = pd.read_csv('../data/online_order.csv',encoding = 'gbk',dtype={'customer':str,'order':str})
df.dtypes
```

创建对象，按照星期进行分组

```python
wgroup = df.groupby('weekday')
#调用
wgroup.mean()
```

计算不同清洗商品数量的总和

```python
wgroup.sum()['total_items']
```

创建对象，按照用户和星期

```python
cwgrou = df.groupby(by=['customer','weekday'])
```

计算不同的用户周一到周天的订购商品数量的总和

```python
cwgrou.sum()['total_items'].head(50)
```

### 2.聚合函数使用
- 使用聚合函数agg
- agg是一个作用于series或者DataFrame的函数，主要目的是针对分组后的对象，使用相关函数进行计算

```python
#按照周一到周天分组，创建对象
msgroup = df.groupby('weekday')
#计算均值、总和
msgroup.agg([np.mean,np.max]).head(20)
```

对2个变量分别计算不同的统计量

```python
msgroup.agg({'total_items':np.sum,'Food%':[np.mean,np.median]})
```

对其它数据进行汇总

```python
df[['total_items','Food%','Drinks%']].agg([np.sum,np.mean])
```

### 3. 分组对象于apply函数

apply函数，0表示行，1表示列

```python
ogroup = df.groupby('weekday')
#做聚合
ogroup.apply(np.mean)[['total_items','Food%']]
#不同类型的商品占比
var_name = ['Food%','Fresh%', 'Drinks%', 'Home%', 'Beauty%', 'Health%', 'Baby%','Pets%']
```

计算每行总和

```python
df[var_name].apply(np.sum,axis=0)
```

计算每列

```python
df['sum'] = df[var_name].apply(np.sum,axis=1)
```

计算食物在订单总价中占比 - 生鲜类食物在订单总中占比

```python
df[var_name].apply(lambda x : x[0] - x[1],axis = 1)
```

### 4.透视表与交叉表

```python
dff = pd.read_csv('../data/online_order.csv',encoding='gbk',dtype={'customer':int,'order':str})
```

透视表·单个变量，margin=True是否需要总计，按照周一到周天计算购买的商品数量总数和次数

```python
pd.pivot_table(data=df,index='weekday',values='total_items',aggfunc=[np.sum,np.size],margins=True,margins_name='总计')
```

交叉表·交叉表更多用于计算分组频率

使用交叉表；是一种计算分组频数的特殊透视表；不同的星期，不同的折扣交叉表

```python
pd.crosstab(index=df['weekday'],columns=df['discount%'],margins=True)
```

按照行进行汇总，计算频数占比;    index表示计算行百分比，columns表示计算列百分比

```python
pd.crosstab(index=df['weekday'],columns=df['discount%'],margins=True,normalize='all')
```

index表示计算行百分比，columns表示计算列百分比

```python
pd.crosstab(index=df['weekday'],columns=df['discount%'],margins=True,normalize='columns')
```



















