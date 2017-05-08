
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
此参数用于在解压或压缩时为文件添加路径。如，压缩时可将`file.txt`文件以`backup/file.txt`路径
放到压缩包中；解压时可将`data/file.txt`解压到`folder/data/file.txt`
（译注：`folder`没有出现在下面例子中）。

```php
$list = $archive->create(
    "file.txt,image.gif",
    PCLZIP_OPT_ADD_PATH, "backup"
);
```

此参数可用于以下方法：
- create()
- add()
- extract()




## PCLZIP_OPT_REMOVE_PATH
此参数用于在解压或压缩时去掉一些前缀路径。
如，压缩时可将`/usr/local/user/test/file.txt`压缩为`test/file.txt`；
解压时将压缩包内的`folder/data/file.txt`解压为`data/file.txt`。

参数值：一个字符串。

```php
$list = $archive->add(
    "/usr/local/user/test/file.txt",
    PCLZIP_OPT_REMOVE_PATH, "/usr/local/user"
);
```

此参数可用于以下方法：
- create()
- add()
- extract()
- extractByIndex()

注意：在同一方法调用中，若已指定`PCLZIP_OPT_REMOVE_ALL_PATH`参数，则本参数将自动被忽略。






## PCLZIP_OPT_REMOVE_ALL_PATH
此参数用于在解压或压缩时，去掉文件的所有路径（译注：即所有文件都放在根目录下，没有任何子目录）。

使用此参数时无需指定任何路径，有时简单理解为`PCLZIP_OPT_REMOVE_PATH`参数的批量操作。
但此参数使用时需注意，应保证不同子目录中不存在同名的文件。

本参数没有参数值。

```php
$list = $archive->create(
    "data/file.txt images/image.gif",
    PCLZIP_OPT_REMOVE_ALL_PATH
);
// 上面会从 'data/file.txt' 中去掉 'data/'
// 并从 'images/image.gif' 中去掉 'images/'
```

此参数可用于以下方法：
- create()
- add()
- extract()
- extractByIndex()







## PCLZIP_OPT_SET_CHMOD
此参数用于在解压后对文件的权限进行修改。

在类 *NIIX 系统中，文件管理系统和文件拥有者一般不允许对系统的所有文件进行访问。
特别的，启动 PHP 进程的用户会对解压出来的文件进行保护，一般不允许其他用户对
其进行访问。

本参数会尝试对解压出来的文件的权限进行修改，如允许写访问，但不修改文件的拥有者。

参数值：一个八进制值（如 0777）

```php
$list = $archive->extract(PCLZIP_OPT_SET_CHMOD, 0777);
```

此参数可用于以下方法：
- extract()
- extractByIndex()






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

