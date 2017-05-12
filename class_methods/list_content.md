
# 方法：PclZip::listContent
英文原文：http://www.phpconcept.net/pclzip/user-guide/54

## 概述
本方法用于列出压缩包中的文件和目录信息。



## 用法
```php
PclZip::listContent()
```




## 返回值
可能的返回值：
- 0：出错时会返回 0 
- 一个数组：各个文件的属性信息（更多见`返回值`页面）



## 描述
略。



## 示例
```php
include_once('pclzip.lib.php');

$zip = new PclZip("test.zip");

if (($list = $zip->listContent()) == 0) {
    die("Error : " . $zip->errorInfo(true));
}

for ($i=0; $i<sizeof($list); $i++) {
    for(reset($list[$i]); $key = key($list[$i]); next($list[$i])) {
      echo "File $i / [$key] = " . $list[$i][$key] . "";
    }
    echo "";
} 
```

上面示例会得到下面输出：
```
File 0 / [filename] = data/file1.txt
File 0 / [stored_filename] = data/file1.txt
File 0 / [size] = 53
File 0 / [compressed_size] = 36
File 0 / [mtime] = 1010440428
File 0 / [comment] = 
File 0 / [folder] = 0
File 0 / [index] = 0
File 0 / [status] = ok
File 1 / [filename] = data/file2.txt
File 1 / [stored_filename] = data/file2.txt
File 1 / [size] = 54
File 1 / [compressed_size] = 53
File 1 / [mtime] = 1011197724
File 1 / [comment] = 
File 1 / [folder] = 0
File 1 / [index] = 1
File 1 / [status] = ok
```