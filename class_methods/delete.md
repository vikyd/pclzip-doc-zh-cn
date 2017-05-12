
# 方法：PclZip::delete
英文原文：http://www.phpconcept.net/pclzip/user-guide/58

## 概述
本方法用于删除压缩包中的全部或部分文件。



## 用法
```php
PclZip::delete([可选参数])
```


## 参数
## 参数
- 支持的可选参数
  - PCLZIP_OPT_BY_NAME
  - PCLZIP_OPT_BY_EREG
  - PCLZIP_OPT_BY_PREG
  - PCLZIP_OPT_BY_INDEX  

更多见`可选参数`页面。




## 返回值
可能的返回值：
- 0：出错时会返回 0 
- 一个数组：还剩在压缩包中的文件




## 描述
本方法用于删除压缩包中的全部或部分文件。

以下参考可用于过滤不想删除的文件：
- PCLZIP_OPT_BY_NAME
- PCLZIP_OPT_BY_EREG
- PCLZIP_OPT_BY_PREG
- PCLZIP_OPT_BY_INDEX




## 示例
```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->delete();
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，压缩包中的所有文件都会被删除。


```php
require_once('pclzip.lib.php');
$archive = new PclZip('archive.zip');
$v_list = $archive->delete(PCLZIP_OPT_BY_INDEX, '1-3,5,8-10');
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，索引在 1 至 3 和 5 至 8 范围中的文件或目录将被删除。
可通过`listContent()`方法获取文件的索引。

注意：索引中的`目录`也有自己的索引号，因此删除一个目录后，目录里面的文件不会被删除。



```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->delete(PCLZIP_OPT_BY_EREG, 'txt$');
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，压缩包中所有`txt`格式的文件都被删除。






