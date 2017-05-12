
# 常见问题
英文原文：http://www.phpconcept.net/pclzip/faq


### 问题 1：为什么在创建一个 zip 包时，运行结束后仅留下一个空的 zip 包？
这可能是因为 PHP 中并没有启用 zlib 扩展。
PclZip 基于 zlib 进行压缩。
在 v1.1 后面的版本中会添加自动检测 zlib 是否已开启。



### 问题 2：为什么我不能用 WinZip 软件打开 PclZip 创建的 zip 包？
WinZip < 6.0 的版本不支持 PclZip 创建的 zip 包，
WinZip >= 6.0 的版本支持。

译注：估计我们现在都很少用 WinZip 了吧，至少译者使用的 7-Zip v16 可以支持 PclZip 的 zip 包。



### 问题 3：在使用 PclZip 提取（创建）zip 时，PHP 在运行结束之前一直挂起（译注：即一直等待的意思），没有任何错误提示...
PclZip 的运行性能与下面两个 PHP 配置参数有关：
- `memory_limit`：http://php.net/manual/zh/ini.core.php#ini.sect.resource-limits
  - memory_limit 是一次 PHP 运行中的最大内存限制，在`php.ini`中配置，默认值：8MB。PclZip 解压或压缩文件时，若碰到较大的文件时，
  不可能全部在内存中操作，而会使用系统的临时文件。PclZip 是否使用临时的依据正是 PHP 的 memory_limit 参数。
- `set_time_limit`：http://php.net/manual/zh/function.set-time-limit.php
  - 当 PclZip 在处理较大的文件或文件数量特别大的情况下，需要较多的时间来处理。若时间过长会因为超时而运行失败，
  set_time_limit 正是配置此值的参数，默认值：30s。



### 问题 4：为什么有时我用 PclZip 解压一个压缩包时，没解压出任何文件，也没看到任何错误提示？
PclZip 是支持错误提示的机制的，详见`异常处理`页面。

注意：你必须在 PclZip 方法执行完后才能通过一定方法获取到错误信息。

PclZip支持所谓的`错误恢复`机制，即在碰到部分错误时，并不会停止整个操作。
解压时，当压缩包内其中一个文件解压失败后，并不会终止后续其他文件的解压。

那如何知道这么多文件哪个解压成功？哪个失败（如访问权限、文件已存在、目录名冲突等原因）？
你需要了解返回的结果数组。

示例：

```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
if (($v_result_list = $archive->extract()) == 0) {
    die("Error : " . $archive->errorInfo(true));
}

echo "<pre>";
var_dump($v_result_list);
echo "</pre>";
```

从 v2.2 开始，你可以设置在解压时只要一个文件解压失败就立即停止后续文件的解压。
对应的设置参数是`PCLZIP_OPT_STOP_ON_ERROR`。

```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
if (($v_result_list = $archive->extract(PCLZIP_OPT_STOP_ON_ERROR)) == 0) {
    die("Error : " . $archive->errorInfo(true));
}

echo "<pre>";
var_dump($v_result_list);
echo "</pre>";
```




### 问题 5：我如何可以将当前整个目录（含子目录）压缩为一个 zip 包？

PclZip 并不支持直接输入`./`来指定当前目录，但你可以这么做：

```php
require_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_dir = getcwd(); // or dirname(__FILE__);
$v_remove = $v_dir;
// 若在 Windows 中，为了支持 C: 根目录，你需要添加下面 3 行
// 若在 Linux 下，则忽略下面 3 行
if (substr($v_dir, 1,1) == ':') {
    $v_remove = substr($v_dir, 2);
}

$v_list = $archive->create($v_dir, PCLZIP_OPT_REMOVE_PATH, $v_remove);
if ($v_list == 0) {
    die("Error : ".$archive->errorInfo(true));
}
```

