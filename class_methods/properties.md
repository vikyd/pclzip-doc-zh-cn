
# 方法：PclZip::properties
英文原文：http://www.phpconcept.net/pclzip/user-guide/56

## 概述
本方法用于获取压缩包的属性信息。



## 用法
```php
PclZip::properties()
```




## 返回值
可能的返回值：
- 0：出错时会返回 0 
- 一个包含压缩包属性信息的数组



## 描述
获取到的压缩包属性信息包括：
- nb：压缩包中的文件+目录数量
- comment：压缩包的注释文字
- status：压缩包的状态（暂只有`OK`状态）

