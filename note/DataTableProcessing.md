# 数据表处理

### 1.数据筛选方法

```python
import pandas as pd
import numpy as np
df = pd.read_csv('../data/baby_trade_history.csv',encoding='utf-8',dtype={'user_id':str})
```

查看数据

```python
df.info()
#查看前5行
df.head(5)
#查看后5行
df.tail(5)
#查看变量名称
df.columns
#第2到4行
df['user_id'][1:5]
```

loc和iloc使用
loc：根据位置；
iloc：根据索引；

```python
#选择行位置
df.loc[3:4]
#选择某两列
df.loc[:,['user_id','buy_mount']]
#loc在这里选择行位置
df.loc[1:3,['user_id','buy_mount']]
#多个条件选择
df.loc[(df.user_id=='786295544') | (df.user_id=='444069173'),['user_id','buy_mount','day']]
#按照索引来选择第2列到第4列
df.iloc[:,1:5]
#按照位置来选择第1列和第3列
df.iloc[:,[0,2]]
#按照索引，选择第4行，第2列和第3列数据，这里的3代表的是索引标签，而不是位置；
df.iloc[3,[1,2]]
#按照索引，选择第3行到第7行，第2列和第3列数据
df.iloc[2:8,[1,2]]
```

### 2.数据增加和删除

- 增加行在dataframe中不常用；
- 可以使用append方法，在dataframe中；
- 可以使用insert方法
- df.insert(位置，变量名称，值)
- 将auction_id取出来，放在一列

```python
#增加一列，购买量，购买量超过3的为高，低于3的为低
df['购买量'] = np.where(df['buy_mount'] > 3,'高','低')
df.insert(0,'auction_id',auction_id)
```

删列

- 删除这两列，加inplace代表是否在原数据上操作，1表示列；
- 同时删除多个变量，需要以列表的形式
- 注意inplace = True，代表是否对原数据操作，否则返回的是视图，并没有对原数据进行操作
- labels ：删除的数据；
- axis：作用轴，0-行，1-列；

```python
df.drop(labels=['property','购买量'],axis=1,inplace=True)
#删除索引标签3和4对应的行
df.drop(labels=[3,4],inplace=True,axis=0)
#删除索引名称1到10,注意range迭代器产生的是1到10
df.drop(labels=range(1,11),axis=0,inplace=True)
```

### 3. 数据修改和查找

```python
df3 = pd.read_csv('../data/sam_tianchi_mum_baby.csv',encoding='utf-8',dtype=str)

# 将gender为0的改为女性，1改为男性，2改为未知
df3.loc[df3['gender']=='0','gender']='女性'
df3.loc[df3['gender']=='1','gender']='男性'
df3.loc[df3['gender']=='2','gender']='未知'
```

修改列标签和行索引名称

```python
df3.rename(columns = {'user_id':'用户ID','birthday':'出生日期','gender':'性别'},inplace=True)
```

重制索引

```python
df3.reset_index(drop=True,inplace=True)
```

查询

条件查询

```python
#条件查询:大于3
df[df.buy_mount > 3]
#条件查询，没大于3,~表示非
df[~(df.buy_mount>3)]
```

多条件查询

```python
df[(df.buy_mount>3)& (df.day>20131008)]
```

between

```python
#使用between，inclusive=True表示包含
df[df['buy_mount'].between(4,10,inclusive=True)]
```

isin

```python
#使用pd.isin()方法--包含
df[df['auction_id'].isin([41161316434,10795639825])]
```

### 4.数据整理
横向堆叠在数据清洗中不常用，纵向堆叠可以理解为把不同表，字段名称一样，整合在一起

```python
import xlrd
workbook = xlrd.open_workbook('../data/meal_order_detail.xlsx')
#返回所有sheet的列表
sheet_name = workbook.sheet_names()
sheet_name
['meal_order_detail1', 'meal_order_detail2', 'meal_order_detail3']

order1 =pd.read_excel('../data/meal_order_detail.xlsx',sheet_name='meal_order_detail1')
order2 = pd.read_excel('../data/meal_order_detail.xlsx',sheet_name='meal_order_detail2')
order3 = pd.read_excel('../data/meal_order_detail.xlsx',sheet_name='meal_order_detail3')
```

忽略原来的索引ignore_index=False

```python
order = pd.concat([order1,order2,order3],axis=0,ignore_index=False)
```

通过循环的方式进行合并

```python
basic = pd.DataFrame()
for i in sheet_name:
    basic_i = pd.read_excel('../Data/meal_order_detail.xlsx',header=0,sheet_name=i,encoding='utf-8')
    basic = pd.concat([basic,basic_i],axis=0)
```

关联·关联字段必须类型一致

```python
#交易数据
dfj = pd.read_csv('../data/baby_trade_history.csv',encoding='utf-8',dtype={'user_id':str})
#婴儿信息
dfy = pd.read_csv('../data/sam_tianchi_mum_baby.csv',encoding='utf-8',dtype=str)
```

内在连接

```python
dfjy = pd.merge(left=dfj,right=dfy,how='inner',left_on='user_id',right_on='user_id')
dfjy.head(2)
```

### 5.层次化索引

```python
#将数据位置第4列和第一列当成索引
df5 = pd.read_csv('../data/baby_trade_history.csv',encoding='utf-8',dtype={'user_id':str},index_col=[3,0])
```

第一层引用

```python
df5.loc[28]
```

第二层引用

```python
df5.loc[28].loc[[82830661,532110457]]
```

直接引用两层，df5.loc[(a,b):]   a和b分别代表第一层和第二层的索引，接受tuple

```python
#第二层索引，多个选择
df5.loc[(28,[82830661,532110457]),:]
#第二层索引，选择2个变量
df5.loc[(28,[82830661,532110457]),['auction_id','cat_id']]
```

























