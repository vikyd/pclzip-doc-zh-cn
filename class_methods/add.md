
# 方法：PclZip::add
英文原文：http://www.phpconcept.net/pclzip/user-guide/57

## 概述
本方法用于添加文件/目录到压缩包中。

（PclZip >= 1.1）


## 用法
```php
PclZip::add($filelist, [可选参数])
```



## 参数
- $filelist：文件列表，可以是以下内容
  - 一个数组，每个数组项是一个文件或目录名
  - 一个字符串：一个文件或目录名
  - 一个字符串：一堆文件或目录名，并以英文逗号`,`分隔
- 支持的可选参数
  - PCLZIP_OPT_REMOVE_PATH
  - PCLZIP_OPT_REMOVE_ALL_PATH
  - PCLZIP_OPT_ADD_PATH
  - PCLZIP_CB_PRE_ADD
  - PCLZIP_CB_POST_ADD
  - PCLZIP_OPT_NO_COMPRESSION
  - PCLZIP_OPT_COMMENT
  - PCLZIP_OPT_ADD_COMMENT
  - PCLZIP_OPT_PREPEND_COMMENT
  - PCLZIP_OPT_TEMP_FILE_THRESHOLD
  - PCLZIP_OPT_TEMP_FILE_ON
  - PCLZIP_OPT_TEMP_FILE_OFF

更多见`可选参数`页面。



## 返回值
可能的返回值：
- 0：出错时会返回 0 
- 一个数组：各个文件的属性信息（更多见`返回值`页面）



## 描述
本方法用于将`$filelist`参数中指定的文件/目录添加到压缩包中。
若是目录，则目录中的所有内容包括子目录结构都会添加到压缩包中。

若压缩包中已存在某个文件，则再次添加同名文件到压缩包时，
PclZip 并不会自动覆盖已有文件，而是在压缩包内的相同目录中同时存在 2 个同名的文件。
（译注：暂未找到一个参数，可以让 PclZip 自动覆盖压缩包内已存在的文件）

可选参数可用于：
- 修改文件在压缩包内的目录路径
- 不压缩文件，直接添加到压缩包中
- 为整个压缩包添加注释文字




## 示例
```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->add('file.txt,data/text.txt,folder/');
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，会将以下文件/目录添加到压缩包中：
- file.txt
- data/text.txt
- folder/


```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->add(
    'dev/file.txt,dev/text.txt',
    PCLZIP_OPT_ADD_PATH, 'install',
    PCLZIP_OPT_REMOVE_PATH, 'dev'
);
if ($v_list == 0) {
    die("Error : ".$archive->errorInfo(true));
} 
```

上面示例中，最终的效果是：

文件系统中：
- `dev/file.txt`
- `dev/text.txt`

添加到压缩包后的目录结构：
- `install/file.txt`
- `install/text.txt`
