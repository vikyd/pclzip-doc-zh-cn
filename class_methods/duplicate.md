
# 方法：PclZip::duplicate
英文原文：http://www.phpconcept.net/pclzip/user-guide/60

## 概述
本方法用于复制压缩包。

（PclZip >= 1.2）



## 用法
```php
PclZip::duplicate($archive_filename)
```



## 参数
- $archive_filename：将要被复制的压缩包



## 返回值
- 0：出现错误
- 1：复制成功



## 描述
本方法用于仅将参数`$archive_filename`中的压缩包复制为本对象刚创建的压缩包中。

若本对象已关联了一个已存在的压缩包，则会先删除这个压缩包，再复制。



