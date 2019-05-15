---
title: python pandas
tags: data analysis
---

# pandas数据结构
## Series
## DataFrame
## Index
# 基本功能
## 重新索引
## 丢弃指定轴上的项
## 索引、选取和过滤
## 算术运算和数据对齐
## 函数应用和映射
## 排序和排名
## 带有重复值的轴索引
# 汇总和计算描述统计
注： 约简和汇总统计NA值会自动排除，DataFrame和Series运算则不会
## 先关系数和协方差
## 唯一值、值计数以及成员资格
## 处理缺失数据
## 滤除缺失数据
## 填充缺失数据
# 层次化索引
## 重排分级顺序
## 根据级别汇总统计
## 使用DataFrame的列（为索引）
set_index
# 数据的加载、存储与文件格式
## 读取文本格式
### csv read_csv
### read_table
### read_json
### read_pickle
### read_hdf
### read_excel
### read_html
### read_sql
### read_frame
## 写文本格式
### to_csv
### to_json
### to_pickle
### to_hdf
### to_excel
### to_html
### to_sql
### to_frame
## python 处理文本的库
### csv
import csv
f = open('xx/xx')
reader = csv.reader(f)
with open('mydata.csv', 'w') as f:
wirter = csv.writer(f, dialect=my_dialect)
wirter.writerow(('one','two','three'))
### json
import json
json.loads()
asjson = json.dumps(result)
### XML和HTML: web信息收集
from lxml.html import parse
from urllib2 import urlopen #获取整个html
import requests #获取response模块
### lmxl.objectify解析XML
from lxml import objectify
from StringIO import StringIO
### ExcelFile
import openpyxl
### 处理数据库
mongodb
import pymongodb
#数据规整化：清理、转换、合并、重塑
## 合并数据集
pandas.merge
pandas.concat
pandas.combine_first
## 重塑和轴向旋转
df.stack()
df.unstack()
df.pivot('date', 'item', 'value')
## 数据转换
df.duplicated()
df.drop_duplicated()
df.map()
df.replace()
df.rename()
### 离散化和面元划分
pd.cut(ages, bins)
pd.qcut(data, [0,0.1, 0.5, 0.9, 1])
### 检测和过滤异常值
data[(np.abs(data)> 3).any(1)]
### 排列和随机采样
sampler=np.random.permutations(5)
sampler=np.random。randint(0, len(bag), size=10)
df.take()
### 计算指标/哑变量
dummies = pd.get_dummies(df['key'], prefix='key')
## 字符串操作
### 字符串对象方法
count
endswith，statswith
join
index
find
rfind
replace
strip、rstrip、lstrip
split
lower、upper
ljust、rjust
### 正则表达式
import re
regex = re.compile('\s+')
regex.split(text)
. 匹配任何字符（除了换行）
^ 排除某个字符（匹配行开始）
$ 匹配行的结尾
*-0次或多次
+-1次或多次
?-0次或1次
{m,n} 最少m次，最多n次
[]
\ 转义字符
() 子模式
| or
regex.match()
regex.search()
regex.sub()
regex.subn()
regex.findall()
regex.finditer
### pandas中矢量化的字符串函数
矢量化的字符串方法
cat
contains
count
endswith、startwith
findall
get
join
len
lower
upper
match
pad
center
repeat
replace
slice
split
strip、rstrip、lstrip

### 透视表和交叉表






