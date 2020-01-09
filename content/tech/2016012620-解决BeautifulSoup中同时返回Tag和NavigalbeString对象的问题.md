+++
title = "解决BeautifulSoup中同时返回Tag和NavigalbeString对象的问题"
date = "2016-01-26T20:00:00+08:00"
slug = "2016012620"
+++

在BeautifulSoup中，用contents或children遍历子节点的时候，如果节点下存在字符串，则会同时获取`Tag`和`NavigalbeString`对象。这是一个非常坑爹的特性，一方面通常获取子节点主要是得到`Tag`，另一方面，bs已经提供了`strings`及`stripped_strings`单独获取节点下的字符串，这里就是多此一举。

下面以`contents`为例，来看看这个问题的具体情况并给出两种解决方案。

## 一个例子

假设有下面这个xml：

```xml
<tab>
<tabletitle>
<title name="公司名称"/>
<title name="所属行业"/>
<title name="主营业务"/>
<title name="董事长"/>
<title name="最终控制人"/>
</tabletitle>
<tablelist>
<col con="许继电气股份有限公司"/>
<col con="电气设备"/>
<col con="从事电力系统二次设备和一次设备的研制、销售"/>
<col con="许继集团有限公司"/>
<col con="国务院国有资产监督管理委员会"/>
</tablelist>
<tablelist>
<col con="中国南方航空股份有限公司"/>
<col con="机场航运"/>
<col con="提供国内、港澳台地区及国际航空客运、货运及邮运服务"/>
<col con="中国南方航空集团公司"/>
<col con="国务院国有资产监督管理委员会"/>
</tablelist>
</tab>
```

现在需要把数据转成普通的表格，方便存入excel中，代码如下：

```python
from bs4 import BeautifulSoup

from_str = '''
<tab>
<tabletitle>
<title name="公司名称"/>
<title name="所属行业"/>
<title name="主营业务"/>
<title name="董事长"/>
<title name="最终控制人"/>
</tabletitle>
<tablelist>
<col con="许继电气股份有限公司"/>
<col con="电气设备"/>
<col con="从事电力系统二次设备和一次设备的研制、销售"/>
<col con="许继集团有限公司"/>
<col con="国务院国有资产监督管理委员会"/>
</tablelist>
<tablelist>
<col con="中国南方航空股份有限公司"/>
<col con="机场航运"/>
<col con="提供国内、港澳台地区及国际航空客运、货运及邮运服务"/>
<col con="中国南方航空集团公司"/>
<col con="国务院国有资产监督管理委员会"/>
</tablelist>
</tab>'''

soup = BeautifulSoup(from_str, 'lxml-xml')
tablelist = soup.findAll('tablelist')
for i in tablelist:
    for j in i.contents:
        print(j['con'], end='\t')


```

运行上面代码会发现`print`语句报错：

```python
TypeError: string indices must be integers
```

提示`string`类型的索引，必须是整数。于是将`print`语句改为

```python
print(type(j))
```

看下`j`的类型，发现同时有`Tag`和`NavigableString`。表面上看xml中没有字符串，但其实在每个标签之间有个换行符（只有换行符时也获取了`NavigalbeString`就更坑爹了），所以contents遍历的时候，获取了`NavigableString`类型的对象。

解决这个问题，有两个比较常见的方法。

## 两种方法

### 方法一：使用`isinstance`判断类型

在循环中加一个类型判断，过滤掉`NavigableString`类型的子节点，即可输出正常的结果，代码如下：

```python
for i in tablelist:
    for j in i.contents:
        if not isinstance(j, NavigableString):  ## 类型判断
            print(j['con'], end='\t')
```

注意这种方法需要在前面加上`from bs4 import NavigableString`导入相应的模块。

### 方法二：使用`findAll(True)`

另一种是用`findAll()`方法，只要加上`True`参数，就会返回所有`Tag`类型的子节点：

```python
for i in tablelist:
    for j in i.findAll(True):  ## True参数只返回Tag子节点
        print(j['con'], end='\t')
```

两种方法都可以实现只获取`Tag`类型子节点，LZ更喜欢第2种，少写一层缩进，少写一个import，更重要的是可以少一些循环次数。感觉以后完全不需要`contents`和`children`两个属性了。


