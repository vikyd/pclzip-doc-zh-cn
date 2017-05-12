
# 方法：PclZip::PclZip
英文原文：http://www.phpconcept.net/pclzip/user-guide/52

## 概述
本方法是 PclZip 对象的构造函数。



## 用法
```php
PclZip($zip_filename)
```



## 参数
- $zip_filename：PKZIP 压缩包的文件名
 


## 描述
本方法创建一个表示 PKZIP 的 PclZip 对象，只需一个文件名参数，并执行一些检查，没有其他操作。

本构造函数会检查 PHP 的`zlib`扩展是否可用。若不可用，将终止执行，并产生错误信息。



## 示例
```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
if ($archive->create('file.txt data/text.txt folder/') == 0) {
    die('Error : ' . $archive->errorInfo(true));
}
```