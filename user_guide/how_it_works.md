
# 如何使用
英文原文：http://www.phpconcept.net/pclzip/user-guide/18


## PKZIP 压缩包的内部表示方式
每个 PKZIP 压缩包都由一个 PclZip 对象表示。
当使用 PclZip 对象创建一个 PclZip 压缩包时，需绑定压缩包的名字。
此时，PclZip 不会检查压缩包，也不可读，甚至压缩包还不存在。

```php
require_once('pclzip.lib.php');
$archive = new PclZip("archive.zip");
```

此压缩包后续将由此 PclZip 对象进行操作控制。

想新建一个不存在的压缩包，应使用`create()`方法，并可传入一些参数，如文件或目录的列表。


## 方法参数与运行参数
每个方法都有自己的参数，这些参数的使用可见每个方法的介绍页面。
这些参数可能是必须也可能是可选的，例如：

```php
require_once('pclzip.lib.php');
$archive = new PclZip('archive.zip');
$v_list = $archive->add('dev/file.txt', PCLZIP_OPT_REMOVE_PATH, 'dev');
```
上面，第 1 个参数`dev/file.txt`是必须参数，`PCLZIP_OPT_REMOVE_PATH`是可选参数。

有些方法只需一些可选参数：
```php
$list = $archive->extract(
    PCLZIP_OPT_PATH, "folder",
    PCLZIP_OPT_REMOVE_PATH, "data",
    PCLZIP_CB_PRE_EXTRACT, "callback_pre_extract",
    PCLZIP_CB_POST_EXTRACT, "callback_post_extract"
);
```
上面参数表示：解压到目录`folder`，如压缩包内有路径是`data`则去掉此路径。

此外，每个文件在解压出来前都会自动调用一个回调函数`callback_pre_extract()`，
此回调函数可用于修改将解压到的路径或跳过不解压此文件。

每个文件在解压完毕后都会自动调用一个回调函数，可用于在解压下一个文件前做一些相关操作。

```php
$list = $archive->extract(
    PCLZIP_OPT_PATH, "folder",
    PCLZIP_OPT_REMOVE_ALL_PATH
);
```
上面例子的作用是解压所有文件到`folder`目录，并且原来在压缩包内的所有文件的路径都将被去掉
（译注：即所有层次的文件都将被解压到`folder`根目录）。此示例中，可无需指明针对哪个文件的路径。

以上这些示例大致演示了如何使用参数。基于此方式使用参数，可提供更好的功能（代价是稍增加了复杂度）。
这样的参数设计，也简化了每个方法函数的设计。

更详细的参数使用方式，可见于`参数选项`页面。并且，每个方法的参数描述中都将由详细的说明。



## 返回值
每个方法的返回值可能不一样，详细见每个方法的介绍页面。

但大部分方法都在出错时返回`0`（以及错误标记），在成功时返回一个描述每个文件的数组。、
此数组的每项包括有一个文件或目录的属性信息和最后操作该文件的状态。

下表列出了每个文件包含的信息：
- filename：文件名。添加到压缩包时，此变量代表调用方法是的参数；解压出来时，代表解压后的文件名
- stored_filename：压缩包内存储的文件名
- size：文件的实际大小
- compressed_size：压缩后的文件大小（不包含头部）
- mtime：文件的最后修改日期和时间（UNIX 时间戳）
- comment：此文件的注释文字
- folder：true | false：表示文件或是目录
- index：文件在压缩包内的索引（当已设置时）
- content：文件解压后的内容。此变量仅当已设置`PCLZIP_OPT_EXTRACT_IN_STRING`参数时才存在
- status：操作的结果状态（根据每个操作而不同）
    - ok：成功 OK！
    - filtered：此文件/目录已被用户设置不解压处理
    - already_a_directory：此文件没有解压，因为一个同名的目录已存在
    - newer_exist：此文件没有解压，因为已有同名文件存在，且该文件已被写保护
    - write_protected：此文件没有解压，因为已有同名解压出来了（the file was not extracted because a more recent file exists.）
    - path_creation_fail：此文件没有解压，因为文件目录没有创建成功
    - write_error：此文件没有解压，因为出现写错误
    - read_error：此文件没有解压，因为出现读错误
    - invalid_header：此文件没有解压，因为头信息不正确
    - skipped：此文件没有解压或添加到压缩包，因为用户已在回调函数中跳过此文件
    - filename_too_long：文件名太长而没有被添加到压缩包中（最大 255 字符）（v1.3开始引入）













