
# 介绍
PclZip 是一个 PHP 库，用于压缩或解压 zip 压缩包。
 
本库定义了一个 PclZip 类。一个此类的对象表示一个 zip 压缩包。本文将列举操作 zip 的一些方法。

注意：只有 public 方法是持续维护的（若不维护，也会在前面的版本中提前预告）。private 方法随时都可能在未来版本中被修改，
请不要直接使用 private 方法。

PclZip 只有一个文件：`pclzip.lib.php`，里面包含了本库的所有功能。

在 1.3 版本之前，PclZip 需要两个额外的外部库来辅助追踪 PclZip 本身的堆栈信息和定位错误。

从 1.3 开始，堆栈追踪虽然仍可用，但只能使用特定版本的`pclzip-trace.lib.php`。错误处理也依然直接整合在主文件中
（详见`Handling errors`）。



