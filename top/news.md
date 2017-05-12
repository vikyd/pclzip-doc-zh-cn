
# PclZip 最新消息
英文原文：http://www.phpconcept.net/pclzip/news


译注：
- 原文：http://www.phpconcept.net/pclzip/news
- 原文中描述了最新消息的 PclZip 版本是 v2.6，而不是翻译时看到的 v2.8.2
- 因此可忽略本页内容，因为不够新了



## PclZip 2.5 新特性
PclZip v2.5 引入了一个安全功能，以及修改压缩包内的文件名的功能。
为了实现这些功能，进行了大量代码修改，以保证对压缩包内文件列表的属性的维护，而不只是全局参数的维护。

此版本虽然只允许对文件名进行修改，但代码是朝着可操作压缩包内每个文件
（如将一个字符串作为压缩包内一个新文件，修改文件的日期等）
的方向而修改的。不过暂未对解压提供类似的功能。

用户手册暂时还没更新，不过你可先看看下面关于本功能的快速示例：

```php
$archive = new PclZip("archive.zip");
$list = $archive->create(
    [
        [
            PCLZIP_ATT_FILE_NAME => 'data/file1.txt',
            PCLZIP_ATT_FILE_NEW_FULL_NAME => 'newdir/newname.txt'
        ],
        [
            PCLZIP_ATT_FILE_NAME => 'data/file2.txt',
            PCLZIP_ATT_FILE_NEW_SHORT_NAME => 'newfilename.txt'
        ],
        [
            PCLZIP_ATT_FILE_NAME => 'data/file3.txt'
        ]
    ],
    PCLZIP_OPT_ADD_PATH, 'newpath',
    PCLZIP_OPT_REMOVE_PATH, 'data'
);

if ($list == 0) {
    die("ERROR : '" . $archive->errorInfo(true) . "'");
} 
```

- `PCLZIP_ATT_FILE_NEW_FULL_NAME`的作用是将`data/file1.txt`文件修改成`newdir/newname.txt`，
并且此时全局参数`PCLZIP_OPT_ADD_PATH`和`PCLZIP_OPT_REMOVE_PATH`都将被忽略。
- `PCLZIP_ATT_FILE_SHORT_NAME`的作用是将`file2.txt'`文件修改成`newfilename.txt`，
之后全局参数将继续发挥作用。


GulfTech 提出 PclZip 有一个安全隐患，可在解压文件时进行恶意操作。
PclZip 在解压用户上传的 zip 压缩包时，确实可能会对服务器的系统文件进行修改。
PclZip 支持将文件解压到不同的文件夹。
在 v2.5 中添加了一个控制选项，可用于限制被解压的目录，即不能解压到指定目录之外的地方。
此参数的作用类似于 PHP 自带的`open_basedir`选项。


```php
$archive = new PclZip("archive.zip");
$list = $archive->extract(PCLZIP_OPT_EXTRACT_DIR_RESTRICTION, './base_dir');
if ($list == 0) {
    die("ERROR : '" . $archive->errorInfo(true) . "'");
}
```

上面示例中，PclZip 会将压缩包解压到当前文件夹。若压缩包内的文件解压后是在`base_dir`目录之后，
PclZip 会自动停止压缩，并发出异常信息。

注意：`PCLZIP_OPT_EXTRACT_DIR_RESTRICTION`必须是绝对路径（不能是相对路径）。
例外的是：`./`可以表示当前目录。


> 最后更新于2010年2月7日（星期日）15:19




## PclZip 2.6 新特性

PclZip v2.6 引入了更多的文件层面的特性。现在你可直接将一个字符串添加到压缩包中作为一个新的文件，
而不需要先在文件系统创建文件再添加到压缩包中。

v2.6 还修复了一些已知问题。

用户手册暂时还没更新，不过你可先看看下面关于本功能的快速示例：

```php
$archive = new PclZip("archive.zip");
$v_filename = "new_file.txt";
$v_content = "This is the content of file one\nHello second line";
$list = $archive->create(
    [
        [PCLZIP_ATT_FILE_NAME => $v_filename,
            PCLZIP_ATT_FILE_CONTENT => $v_content
        ]
    ]
);

if ($list == 0) {
    die("ERROR : '" . $archive->errorInfo(true) . "'");
}  
```

上面示例中，`$v_contenu`变量中的字符串将被作为`new_file.txt`文件直接添加到压缩包中。

> 最后更新于2010年2月7日（星期日）15:24




