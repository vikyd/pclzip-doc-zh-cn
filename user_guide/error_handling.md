
# 错误处理
从 v1.3 开始，PclZip 开始增加错误处理功能。
在此之前，只能通过外部的库来处理错误。
增加错误处理功能的主要原因是为了 1 个 PHP 文件就能使用 PclZip。
当然，你依然可以使用外部的库来处理错误，详见`自定义 PclZip`页面。

当一个方法的返回值是一个错误编号（大部分情况是 0）时，
可通过下面方法获取错误的相关信息：
- errorName()：返回错误的名称
- errorCode()：返回错误编号
- errorInfo()：返回错误的描述信息


后面是一些错误处理的示例。


## 获取错误编号
```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
if ($list == 0) {
  die("Unrecoverable error, code " . $archive->errorCode());
}

// 输出：Unrecoverable error, code -6  
```



## 获取错误的名称
```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
if ($list == 0) {
  die("Unrecoverable error '" . $archive->errorName() . "'");
}
 
// 输出：Unrecoverable error 'PCLZIP_ERR_BAD_FORMAT'  
```



## 同时获取错误的名称和编号
```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
if ($list == 0) {
  die("Unrecoverable error '" . $archive->errorName(true) . "'");
}
 
// 输出：Unrecoverable error 'PCLZIP_ERR_BAD_FORMAT(-10)'  
```



## 获取错误的描述信息
```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
if ($list == 0) {
  die("Error : '" . $archive->errorInfo() . "'");
}
 
// 输出：Error : 'Invalid archive structure [code -10]'  
```



## 获取错误的全部信息
```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
if ($list == 0) {
  die("Error  : '" . $archive->errorInfo(true) . "'");
}
 
// 输出：Error : 'PCLZIP_ERR_BAD_FORMAT(-10) : Invalid archive structure'  
```

