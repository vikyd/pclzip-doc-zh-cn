
# 方法：PclZip::create

## 概述
本方法用于根据输入的文件创建一个 PKZIP 压缩包文件。


## 用法
```php
PclZip::create($filelist, [可选参数])
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
  - PCLZIP_OPT_TEMP_FILE_THRESHOLD
  - PCLZIP_OPT_TEMP_FILE_ON
  - PCLZIP_OPT_TEMP_FILE_OFF

更多见`可选参数`页面。



## 返回值
可能的返回值：
- 0：出错时会返回 0 
- 一个数组：各个文件的属性信息（更多见`返回值`页面）



## 描述
本方法会创建一个包含指定文件列表的压缩包。

若文件名是一个目录时，该目录下的所有文件和子目录都会被添加到压缩包中，并且保留原有的目录结构。

可选参数的作用之一是：修改压缩包中的文件路径或文件名，即压缩包中的目录结构可与文件系统中的不一样。
这样可以在压缩包中不暴露文件系统中的目录结构。





## 示例
```php
include_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->create('file.txt,data/text.txt,folder');
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，创建了一个`archive.zip`压缩包，并将`file.txt`、`data/text.txt`文件添加到压缩包中，
压缩包内会自动创建必要的目录结构。


```php
include_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->create(
    'data/file.txt,data/text.txt',
    PCLZIP_OPT_REMOVE_PATH, 'data',
    PCLZIP_OPT_ADD_PATH, 'install'
);
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

上面示例中，最终的效果是：

文件系统中：
- `data/file.txt`
- `data/text.txt`

添加到压缩包后的目录结构：
- `install/file.txt`
- `install/text.txt`







