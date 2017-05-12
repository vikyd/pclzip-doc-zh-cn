
# 方法：PclZip::extract
英文原文：http://www.phpconcept.net/pclzip/user-guide/55

## 概述
本方法用于解压压缩包中的文件和目录。


## 用法
```php
PclZip::extract([可选参数])
```




## 参数
## 参数
- 支持的可选参数
  - PCLZIP_OPT_PATH
  - PCLZIP_OPT_REMOVE_PATH
  - PCLZIP_OPT_REMOVE_ALL_PATH
  - PCLZIP_OPT_ADD_PATH
  - PCLZIP_CB_PRE_EXTRACT
  - PCLZIP_CB_POST_EXTRACT
  - PCLZIP_OPT_SET_CHMOD
  - PCLZIP_OPT_BY_NAME
  - PCLZIP_OPT_BY_EREG
  - PCLZIP_OPT_BY_PREG
  - PCLZIP_OPT_BY_INDEX
  - PCLZIP_OPT_EXTRACT_AS_STRING
  - PCLZIP_OPT_EXTRACT_IN_OUTPUT
  - PCLZIP_OPT_REPLACE_NEWER
  - PCLZIP_OPT_STOP_ON_ERROR
  - PCLZIP_OPT_EXTRACT_DIR_RESTRICTION
  - PCLZIP_OPT_TEMP_FILE_THRESHOLD
  - PCLZIP_OPT_TEMP_FILE_ON
  - PCLZIP_OPT_TEMP_FILE_OFF

更多见`可选参数`页面。




## 返回值
可能的返回值：
- 0：出错时会返回 0 
- 一个数组：已解压的文件的属性信息。
注意：若只有个别文件解压失败，不会导致整个解压过程失败，并不认为是出错，但个别文件的失败状态会记录在列表中。
（更多见`返回值`页面）



## 描述
本方法用于解压压缩包中的全部或部分文件/目录。

以下参数可用于过滤不想解压的项：
- PCLZIP_OPT_BY_NAME
- PCLZIP_OPT_BY_EREG
- PCLZIP_OPT_BY_PREG
- PCLZIP_OPT_BY_INDEX

以下参数可用于：
- PCLZIP_OPT_PATH、PCLZIP_OPT_ADD_PATH：解压到指定的目录中
- PCLZIP_OPT_REMOVE_ALL_PATH：删除压缩包中的所有目录结构，仅保留文件
- PCLZIP_OPT_REMOVE_PATH：只删除部分目录结构

还有：
- PCLZIP_OPT_EXTRACT_AS_STRING：解压文件到 PHP 字符串变量中，而不是解压到文件中
  > 这对于小文件或者文件数较少时比较有用，如解压出 readme 文件
- PCLZIP_OPT_EXTRACT_IN_OUTPUT：解压文件到标准输出中



## 示例
```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
if ($archive->extract() == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，压缩包中的所有文件都解压到当前目录下。


```php
include('pclzip.lib.php');

$archive = new PclZip('archive.zip');
if ($archive->extract(
                      PCLZIP_OPT_PATH, 'data',
                      PCLZIP_OPT_REMOVE_PATH, 'install/release') == 0
   ) {
    die("Error : " . $archive->errorInfo(true));
} 
```

上面示例中，最终的效果是：

压缩包中的目录结构：
- `install/release/file.txt`
- `install/release/text.txt`

解压后，文件系统中：
- `data/file.txt`
- `data/text.txt`

