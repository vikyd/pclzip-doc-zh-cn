
# 简介
http://www.phpconcept.net/pclzip

PHP 库 [PclZip](http://www.phpconcept.net/pclzip) 的中文文档（非官方）。



# 译注：PclZip 是什么？
PclZip 是一个 PHP 库，用于解压或压缩 Zip 压缩包。
 
不同于 PHP 自带的`ZipArchive + getNameIndex`、`zip_open + zip_read + zip_entry_name`（基于 C++），
PclZip 采用纯 PHP 的解决方案，可避免一些 PHP 自带的 zip 方便的 bug（如对 zip 压缩包内含 GBK 编码的文件名的识别问题）。



# 谁在用 PclZip
- [PHPExcel](https://github.com/PHPOffice/PHPExcel/search?utf8=✓&q=pclzip&type=)
- 



# 使用方式
通常有两种使用方式：
1. 直接下载`pclzip.lib.php`，在你的 PHP 文件顶部添加`include_once(__DIR__ . '/pclzip/pclzip.lib.php');`。
1. 使用 Composer 引用 PclZip，引入方式，在命令行执行：`composer require pclzip/pclzip`（非官方，但文件相同）。



# 目录


# 完善
水平有限，难免错漏。

问题意见，欢迎提 PR：https://github.com/Viky-zhang/pclzip-doc-zh-cn.git




# 更新
不定时更新。


# 相关资源
- PclZip 官网：
- PclZip GitHub（非官方）：
- PclZip Compose（非官方）：
- PHPExcel（引用了 PclZip）：
 