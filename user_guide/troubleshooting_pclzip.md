
# 错误定位
英文原文：http://www.phpconcept.net/pclzip/user-guide/24

原文是法文（PclZip 作者未翻译成英文）：http://www.phpconcept.net/pclzip/user-guide/24

PclZip 有两个并存版本：
- 正式版本
- 错误跟踪版本

错误跟踪版本允许你捕获各种异常错误，如：
- 文件权限
- 系统错误
- 系统其它限制等

示例：
```php
require_once('pcltrace.lib.php');
require_once('pclzip-trace.lib.php');

PclTraceOn(2);

$zip = new PclZip('test.zip');
$list = $zip->create("readme.txt");
if ($list == 0) {
    PclTraceDisplay();
    die("Error : " . $zip->errorInfo(true));
}

PclTraceDisplay();
```

有三个方法来设置此功能：
- PclTraceOn()
- PclTraceOff()
- PclTraceDisplay()






## PclTraceOn()
### 概述
此方法用于启用 trace 跟踪信息。
有不同级别可选。


### 用法
```php
PclTraceOn($level, $mode, $filename)
```

### 参数
- `$level`：错误跟踪的级别，范围：1 至 5 级，默认是 1。
- `$mode`：错误跟踪的类型：
  - `normal`：错误跟踪信息会自动输出到 HTML 中（通常是浏览器）
  - `memory`：错误跟踪信息只能通过`PclTraceDisplay()`函数来获取
  - `log`：错误跟踪信息输出到日志文件中（现不可用）
- `$filename`：日志文件的位置（现不可用）



### 返回值
无





## PclTraceOff()

### 概述
此方法用于关闭错误跟踪信息。


### 用法
```php
PclTraceOff()
```


### 参数
无

### 返回值
无





## PclTraceDisplay()

### 概述
此方法用于显示错误跟踪信息。


### 用法
```php
PclTraceDisplay()
```


### 参数
无

### 返回值
无


