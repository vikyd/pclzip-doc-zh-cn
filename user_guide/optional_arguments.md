
# 可选参数
参数有 2 大类：
- 普通变量：传递操作信息到方法
- 回调函数：供用户在 PclZip 处理过程中进行一些操作

`回调函数`可能有些难理解，不过可提供对压缩包更好的操作方式。

这些可选参数是一些常量，实质是一些静态整数。
参数对应的参数值，可以是单个值，也可以是一个列表。
有时某些参数无需设置值，因为参数名本身已包含了必须信息。

下面将列举所有的可选参数：




## PCLZIP_OPT_PATH
此参数指明压缩包的内容将被解压到的目录。
参数值：一个字符串。
```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
```
此参数可用于以下方法：
- extract()
- extractByIndex()




## PCLZIP_OPT_ADD_PATH
```php
$list = $archive->create(
    "file.txt,image.gif",
    PCLZIP_OPT_ADD_PATH, "backup"
);
```


## PCLZIP_OPT_REMOVE_PATH



## PCLZIP_OPT_REMOVE_ALL_PATH



## PCLZIP_OPT_SET_CHMOD



## PCLZIP_OPT_BY_NAME



## PCLZIP_OPT_BY_EREG



## PCLZIP_OPT_BY_PREG



## PCLZIP_OPT_BY_INDEX



## PCLZIP_OPT_EXTRACT_AS_STRING



## PCLZIP_OPT_EXTRACT_IN_OUTPUT



## PCLZIP_OPT_NO_COMPRESSION



## PCLZIP_OPT_COMMENT



## PCLZIP_OPT_ADD_COMMENT



## PCLZIP_OPT_PREPEND_COMMENT



## PCLZIP_OPT_REPLACE_NEWER



## PCLZIP_OPT_EXTRACT_DIR_RESTRICTION



## PCLZIP_OPT_STOP_ON_ERROR



## PCLZIP_OPT_TEMP_FILE_ON



## PCLZIP_OPT_TEMP_FILE_THRESHOLD



## PCLZIP_OPT_TEMP_FILE_OFF



## 回调函数


## IP_CB_PRE_EXTRACT（回调函数）
## PCLZIP_CB_POST_EXTRACT（回调函数）
## PCLZIP_CB_PRE_ADD（回调函数）
## PCLZIP_CB_POST_ADD（回调函数）

