
# 自定义 PclZip
PclZip 可通过配置一些静态常量来进行定制化。
但不建议你修改这些参数，除非你确实需要并且知道你很清楚你所做的是什么。
而且，当 PclZip 升级时，你还需重新配置这些参数。

下面将列举这些参数：
- PCLZIP_READ_BLOCK_SIZE
- PCLZIP_TEMPORARY_DIR
- PCLZIP_SEPARATOR
- PCLZIP_TEMPORARY_FILE_RATIO
- PCLZIP_ERROR_EXTERNAL



## PCLZIP_READ_BLOCK_SIZE
此值表示读取或写入一个文件时的分块大小。
你可根据你的操作系统的环境来修改本参数。

默认值：2048（译注：估计单位是字节）




## PCLZIP_TEMPORARY_DIR
PclZip 在解压或压缩过程中可能会用到临时文件。
这些文件由 PclZip 来生成或删除。
默认的，临时文件的目录就是当前工作目录。这可能会引发一些问题，如当前目录是只读的。

本参数用于指定一个固定的临时文件目录。此目录必须是可读写的，并且必须已存在，PclZip 不会自动新建此目录。
建议使用绝对路径来定义此目录，并避免使用基于 Web 根目录的相对路径等。

此参数值最后必须以`/`结尾。

示例：

```php
define('PCLZIP_TEMPORARY_DIR', '/usr/www/temp/');
include_once('pclzip.lib.php');
```



## PCLZIP_SEPARATOR
在 v1.x 中，文件的分隔符是一个空格（这是一个不好的选择，特别是在 Windows 的路径中），
更好的选择是使用英文逗号`,`。
此常量用于设置该使用什么分隔符。

需注意的是：你修改此参数后，你原来的代码可能需要修改（若已使用了空格作为分隔符的话）。

若想修改为英文分号，示例：
```php
define('PCLZIP_SEPARATOR', ';');
include_once('pclzip.lib.php');
```




## PCLZIP_TEMPORARY_FILE_RATIO
PclZip 压缩或解译文件时可能会自动检测文件大小，并依据一定的阈值来决定是否使用临时文件。

本参数是一个比例值，阈值 = PHP内存大小限制（memory_limit）* 本阈值。

默认值：0.47。

推荐的值：< 0.5。




## PCLZIP_ERROR_EXTERNAL
一个在新版 PclZip 已废弃的参数。

原本作用是：启用外部的错误处理库。






