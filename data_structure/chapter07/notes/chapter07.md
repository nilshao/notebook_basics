# 散列表

## 散列函数

一个把查找表中的关键字映射成该关键字对应的地址的函数

Hash(key) = Addr

要求：

1. 散列函数定义域必须包括所有需要存储的关键字，值域的范围依赖于散列表的大小或地址范围

2. 散列函数计算出来的地址应该能等概率，均匀分布在整个地址空间中，从而减少冲突的发生

3. 散列函数应该尽量简单，在较短时间内计算出关键字对应的散列地址

### 直接定址法

直接取关键字的某个线性函数值作为散列地址，hash(key) = a* key + b

方法简单，不会产生冲突，但是如果关键字分布不连续，会浪费空间

 ### 除留取余（常用）

 hash(key) = key% p

 选好p可以减少冲突的可能，假如散列表表长为m，取一个不大于但是接近或等于m的质数p

 ### 数字分析

 比如一系列数字后几位不同，均匀的

 ### 平方取中法

 适用于关键字的每位取值不均匀或均小于散列地址所需要的位数

 ### 折叠法

 将关键字分割成位数相同的几部分，然后取这几部分的叠加作为散列地址。

 ## 哈希冲突

 冲突不可能绝对避免，如何处理？为产生冲突的关键字寻找下一个空的hash地址

 ### 开放定址法

指可存放新表项的空闲地址既向他的同义词表项开放，又向他的非同义词表项开放



