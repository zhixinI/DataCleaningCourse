# NumpyPandas常用数据结构

## Numpy常用数据结构

### 1.数组组建

```python
import numpy as np
arr1 = np.array([-9,8,7,23])
arr2 = np.array([-9,8,7,23],dtype=float)
arr3 = np.array([[2,4,1,3],[4,6,3,9],[2,5,9,1]])
#使用其它函数创建数组
np.arange(0,10,1)
np.linspace(1,10,10)
np.zeros([3,5])
np.ones([4,2])
```

### 2.数组方法

```python
#数组维度
arr3.ndim
#形状
arr3.shape
#个数
arr3.size
#类型
arr3.dtype
#运算
arr3 + 1
arr3 / 2
data2 = ((3,5,2.4,3),(4,7,2.9,8),(8,3,6,7.8),(2.3,5.6,2.5,1.9))
arr4 = np.array(data2)
arr4
```

### 3.访问

```python
#访问——行索引
arr4[2]
#访问——多行
arr4[0:3]
#访问——多列
arr4[:,1:3]
#访问——多行多列切片
arr4[1:,2:]
#访问值——行索引,列索引
arr4[2,1]
```

### 4.Numpy常用数据清洗函数

- 排序：sort
- 降序：sorted

```python
s = np.array([2,5,1,5,7.8,1,4,7,3,9,7])
#排序
np.sort(s)
#降序
sorted(s,reverse=True)
arrn = np.array([[0,1,3],[4,2,9],[4,5,9],[1,-3,4]])
arrn
#默认排序
np.sort(arrn)
#定行还是列方向,0-行，1-列
np.sort(arrn,axis=0)
np.sort(arrn,axis=1)
```

argsort返回的是排完序以后，在原数据中的索引位置

返回的是数据中,从小到大的索引值

```python
#1最小，对应索引值0，8最大，对应索引值13
s = np.array([1,2,3,4,3,1,2,2,4,6,7,2,4,8,4,5])
np.argsort(s)
```

np.where和np.extract

```python
#满足条件的，赋值为3，不满足的赋值为-1，返回的数据长度和s一样
np.where(s>3,1,-1)
```

np.extract 只会输出满足条件的数据

```python
np.extract(s>3,s)
```

## Pandas常用数据结构

#### Series序列

```python
import pandas as pd
import numpy as np
#构造序列
series1 = pd.Series([2.8,3.01,8.99,8.59,5.18])
series2 = pd.Series([2.8,3.01,8.99,8.59,5.18],index=('a','b','c','d','e'),name='这是一个series')
series3 = pd.Series({'北京':9.8,'上海':9.7,'广州':9.5})
```

### series方法

```python
#series方法
series2.values
#索引
series3.index
```

### Dataframe

构造数据，构造数据框；数据框就是一个二维表结构，数据分析中常用的数据结构

```python
list1 = [['张三',23,'男'],['里斯',18,'女'],['王五',25,'男']]
dff1 = pd.DataFrame(list1,columns=['姓名','年龄','性别'])
dff1

dff2 = pd.DataFrame({'姓名':['张三','里斯','王五'],'':[23,18,25],'年龄':['男','女','男']}) 
dff2

array1 = np.array([['张三',23,'男'],['里斯',18,'女'],['王五',25,'男']])
dff3 = pd.DataFrame(array1,columns=['姓名','年龄','性别'],index=['a','b','c'])
dff3
```

### Dataframe方法

```python
#值
dff3.values
#索引
dff3.index
#列标签
dff3.columns
#类型
dff3.dtypes
#维度
dff3.ndim
#个数
dff3.size
```



