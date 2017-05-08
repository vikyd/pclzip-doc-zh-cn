
# 工作原理

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



## 返回值




















