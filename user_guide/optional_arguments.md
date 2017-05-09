
# 参数目录见页面右上角图标：<i class="fa fa-anchor"></i>

# 可选参数
参数有 2 大类：
- 普通变量：传递操作信息到方法
- 回调函数：供用户在 PclZip 处理过程中进行一些操作

`回调函数`可能有些难理解，不过可提供对压缩包更好的操作方式。

这些可选参数是一些常量，实质是一些静态整数。
参数对应的参数值，可以是单个值，也可以是一个列表。
有时某些参数无需设置值，因为参数名本身已包含了必须信息。

下面将列举所有的可选参数：




## PCLZIP_OPT_PATH
此参数指明压缩包的内容将被解压到的目录。

参数值：一个字符串。

```php
$list = $archive->extract(PCLZIP_OPT_PATH, "extract/folder/");
```

此参数可用于以下方法：
- extract()
- extractByIndex()




## PCLZIP_OPT_ADD_PATH
此参数用于在解压或压缩时为文件添加路径。如，压缩时可将`file.txt`文件以`backup/file.txt`路径
放到压缩包中；解压时可将`data/file.txt`解压到`folder/data/file.txt`
（译注：`folder`没有出现在下面例子中）。

```php
$list = $archive->create(
    "file.txt,image.gif",
    PCLZIP_OPT_ADD_PATH, "backup"
);
```

此参数可用于以下方法：
- create()
- add()
- extract()




## PCLZIP_OPT_REMOVE_PATH
此参数用于在解压或压缩时去掉一些前缀路径。
如，压缩时可将`/usr/local/user/test/file.txt`压缩为`test/file.txt`；
解压时将压缩包内的`folder/data/file.txt`解压为`data/file.txt`。

参数值：一个字符串。

```php
$list = $archive->add(
    "/usr/local/user/test/file.txt",
    PCLZIP_OPT_REMOVE_PATH, "/usr/local/user"
);
```

此参数可用于以下方法：
- create()
- add()
- extract()
- extractByIndex()

注意：在同一方法调用中，若已指定`PCLZIP_OPT_REMOVE_ALL_PATH`参数，则本参数将自动被忽略。






## PCLZIP_OPT_REMOVE_ALL_PATH
此参数用于在解压或压缩时，去掉文件的所有路径（译注：即所有文件都放在根目录下，没有任何子目录）。

使用此参数时无需指定任何路径，有时简单理解为`PCLZIP_OPT_REMOVE_PATH`参数的批量操作。
但此参数使用时需注意，应保证不同子目录中不存在同名的文件。

本参数没有参数值。

```php
$list = $archive->create(
    "data/file.txt images/image.gif",
    PCLZIP_OPT_REMOVE_ALL_PATH
);
// 上面会从 'data/file.txt' 中去掉 'data/'
// 并从 'images/image.gif' 中去掉 'images/'
```

此参数可用于以下方法：
- create()
- add()
- extract()
- extractByIndex()






## PCLZIP_OPT_SET_CHMOD
此参数用于在解压后对文件的权限进行修改。

在类 *NIIX 系统中，文件管理系统和文件拥有者一般不允许对系统的所有文件进行访问。
特别的，启动 PHP 进程的用户会对解压出来的文件进行保护，一般不允许其他用户对
其进行访问。

本参数会尝试对解压出来的文件的权限进行修改，如允许写访问，但不修改文件的拥有者。

参数值：一个八进制值（如 0777）

```php
$list = $archive->extract(PCLZIP_OPT_SET_CHMOD, 0777);
```

此参数可用于以下方法：
- extract()
- extractByIndex()






## PCLZIP_OPT_BY_NAME
此参数用于只解压出某些文件/目录。

（译注：下面例子假设`test.zip`压缩包中至少有这两个文件）

```php
$archive = new PclZip('test.zip');
$rule_list[0] = 'data/file1.txt';
$rule_list[1] = 'data/file2.txt';
$list = $archive->extract(PCLZIP_OPT_BY_NAME, $rule_list);
if ($list == 0) {
    echo "ERROR : " . $archive->errorInfo(true);
}
```

参数值，允许两种情况：
- 一个数组
- 一个字符串，字符串内文件列表用英文逗号隔开

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(PCLZIP_OPT_BY_NAME, "data/file1.txt,data/file2.txt");
if ($list == 0) {
    echo "ERROR : " . $archive->errorInfo(true);
}
```





## PCLZIP_OPT_BY_EREG
此参数用于以正则表达式的方式过滤将被解压的文件/目录。

此参数基于 PHP 自带的 [ereg()](http://php.net/manual/zh/function.ereg.php) 函数。
（译注：ereg 在 PHP7 中将不能用）

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(PCLZIP_OPT_BY_EREG, "txt$");
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_BY_PREG
此参数用于以正则表达式的方式过滤将被解压的文件/目录。

此参数基于 PHP 自带的 [preg_match()](http://php.net/manual/zh/function.preg-match.php) 函数。
（译注：建议使用本参数，而非`PCLZIP_OPT_BY_EREG`）


```php
$archive = new PclZip('test.zip');
$list = $archive->extract(PCLZIP_OPT_BY_PREG, "txt$");
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_BY_INDEX
此参数允许以指定压缩包内的文件索引方式来解压文件/目录。

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(PCLZIP_OPT_BY_INDEX, ['0-4', '2-7', '10-33']);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_EXTRACT_AS_STRING
此参数用于将压缩包内的文件内容直接解压到 PHP 字符串中，而非文件中，不依赖文件系统。

此参数的用途，如：
- 显示一个 readme 文件
- 直接将压缩包内文件的内容直接显示在标准输出中（见参数`PCLZIP_OPT_EXTRACT_IN_OUTPUT`）
- 等

若想将压缩包内的所有文件都解压到 PHP 字符串中，将消耗较大的内存，请防止内存使用超过 PHP 配置的限制。

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(
    PCLZIP_OPT_BY_NAME, "data/readme.txt",
    PCLZIP_OPT_EXTRACT_AS_STRING
);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
  exit;
}
echo $list[0]['content']; 
```





## PCLZIP_OPT_EXTRACT_IN_OUTPUT
此参数用于直接将压缩包内文件的内容直接显示在标准输出中（类似`echo`的功能）。

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(
    PCLZIP_OPT_BY_NAME, "data/readme.txt",
    PCLZIP_OPT_EXTRACT_IN_OUTPUT
);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_NO_COMPRESSION
此参数用于添加不压缩的文件到压缩包。

```php
$archive = new PclZip('test.zip');
$list = $archive->add("data/file.txt", PCLZIP_OPT_NO_COMPRESSION);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_COMMENT
此参数用于为压缩包内的文件设置一个注释字符串，若已有注释，将替换原有注释。

```php
$archive = new PclZip('test.zip');
$list = $archive->create(
    "data", 
    PCLZIP_OPT_COMMENT, "Add a comment"
);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_ADD_COMMENT
此参数用于为压缩包内的文件添加注释字符串，若已有注释，则追加到已有注释后面。

```php
$archive = new PclZip('test.zip');
$list = $archive->add(
    "data", 
    PCLZIP_OPT_ADD_COMMENT, "Add a comment after the existing one"
);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```






## PCLZIP_OPT_PREPEND_COMMENT
此参数用于为压缩包内的文件添加注释字符串，若已有注释，则添加到已有注释之前。

```php
$archive = new PclZip('test.zip');
$list = $archive->add(
    "data", 
    PCLZIP_OPT_PREPEND_COMMENT, "Add a comment before the existing one"
);
if ($list == 0) {
  echo "ERROR : " . $archive->errorInfo(true);
} 
```






## PCLZIP_OPT_REPLACE_NEWER
默认情况下，PclZip 解压文件时，若文件系统中已存在文件，有两种情况：
- 文件系统中的文件时间比压缩包内的文件时间旧，则解压时替换已有文件
- 文件系统中的文件时间比压缩包内的文件时间新，则解压时不替换已有文件

本参数用于替换所有已有文件，不管时间新旧。

这对于还原一个备份时比较有用。

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(
    PCLZIP_OPT_BY_NAME, "data/readme.txt",
    PCLZIP_OPT_REPLACE_NEWER
);
if ($list == 0) {
    echo "ERROR : " . $archive->errorInfo(true);
} 
```





## PCLZIP_OPT_EXTRACT_DIR_RESTRICTION
PclZip 允许解压文件到系统的任意位置，但有人想通过解压到某些位置来覆盖系统文件（如密码文件）或其他文件，
这不安全。

此参数可用于只允许解压到指定目录，而不能解压到其他目录。

此参数可方便的以少量代码减少一些简单的错误。

注意：若要更安全的目录限制，请使用 PHP 的配置选项。

下面例子将不允许任何解压到非`/var/www/data`目录中。

```php
$archive = new PclZip('test.zip');
$list = $archive->extract(PCLZIP_OPT_EXTRACT_DIR_RESTRICTION, "/var/www/data");
if ($list == 0) {
    echo "ERROR : " . $archive->errorInfo(true);
} 
```







## PCLZIP_OPT_STOP_ON_ERROR
默认情况下，解压时，若遇到某些文件不能成功解压，PclZip 并不会自动停止后面文件的解压。
全部解压完毕后，将返回一个解压后的文件列表状态结果（可见于`extract()`页面）。
列表中的每个值都包含每个文件/目录的解压状态。

本参数用于在解压遇到 1 个文件解压失败时就立即停止后续文件的解压。
这可方便你在解压刚出错后进行一些已解压文件的清理或其他操作。


```php
$archive = new PclZip('test.zip');
$list = $archive->extract(
    PCLZIP_OPT_ADD_PATH, "extract_folder/",
    PCLZIP_OPT_STOP_ON_ERROR
);
if ($list == 0) {
  if ($archive->errorCode() == PCLZIP_ERR_ALREADY_A_DIRECTORY) {
    echo "ERROR : File tries to replace a folder !";
  }
} 
```




## PCLZIP_OPT_TEMP_FILE_ON
## PCLZIP_OPT_TEMP_FILE_OFF
## PCLZIP_OPT_TEMP_FILE_THRESHOLD
PclZip 从 v2.7 开始支持压缩或解压时使用临时文件，以更好支持大文件。

大部分情况下，PclZip 都在内存中进行操作，如读取所有文件、压缩文件、写文件到压缩包中。
使用临时文件时，PclZip 在压缩文件时将每次只读取大文件的其中一小块（2048 字节），
并直接将已压缩的小块先写入到临时文件中。当临时文件已写入到压缩包中，则删除该临时文件。

在 v2.8 中，解压时也支持类似的临时文件机制。

当压缩或解压大文件时，PclZip 很快会超过 PHP 配置中设置的内存限制（`php.ini`文件中的`memory_limit`选项）
而导致操作失败。通过使用临时文件，对大文件操作时将提高操作成功率。
但是，相对于在内存中操作，使用临时文件会降低性能。

从 v2.7 开始，PclZip 能自动检测文件的大小，并根据`memory_limit`的选择值，判断是否该采用临时文件方案。
有下面几个参数可配置：
- PCLZIP_OPT_ADD_TEMP_FILE_ON 
  > 全部使用临时文件
- PCLZIP_OPT_ADD_TEMP_FILE_THRESHOLD
  > 超出本参数阈值大小的文件将使用临时文件，否则使用内存
- PCLZIP_OPT_ADD_TEMP_FILE_OFF
  > 全部使用内存，全部不使用临时文件



`PCLZIP_OPT_TEMP_FILE_ON`的示例：
```php
include_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->create(
    'data/image-1.jpg,data/image-2.jpg',
    PCLZIP_OPT_REMOVE_PATH, 'data',
    PCLZIP_OPT_ADD_TEMP_FILE_ON
);
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
} 
```


`PCLZIP_OPT_TEMP_FILE_THRESHOLD`的示例：
```php
include_once('pclzip.lib.php');

$archive = new PclZip('archive.zip');
$v_list = $archive->create(
    'data/image-1.jpg,data/image-2.jpg',
    PCLZIP_OPT_REMOVE_PATH, 'data',
    PCLZIP_OPT_TEMP_FILE_THRESHOLD, 10
);
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
} 
```

`PCLZIP_OPT_TEMP_FILE_OFF`的示例：
```php
include_once('pclzip.lib.php');
ini_set('memory_limit', '180M');
$archive = new PclZip('archive.zip');
$v_list = $archive->create(
    'data/image-1.jpg,data/image-2.jpg',
    PCLZIP_OPT_REMOVE_PATH, 'data',
    PCLZIP_OPT_TEMP_FILE_OFF
);
if ($v_list == 0) {
    die("Error : " . $archive->errorInfo(true));
}
```

此参数可用于以下方法：
- create()
- add()
- extract()
- extractByIndex()

注意：本参数将取代下面这些参数（若存在）
- PCLZIP_OPT_ADD_TEMP_FILE_ON
- PCLZIP_OPT_ADD_TEMP_FILE_THRESHOLD
- PCLZIP_OPT_ADD_TEMP_FILE_OFF





## 回调函数
`回调函数（Call-back）`可作为一种特殊参数，因为其中是一个函数名。
回调函数应严格遵守一些约定，并有不同的分类。
回调函数中可以做任何事，但都应接收约定的参数，并返回约定的值。
回调函数中不应进行一些特殊的操作，如在处理过程中把压缩包删除了，这是不合理的。


## IP_CB_PRE_EXTRACT（回调函数）

```php
function myCallBack($p_event, &$p_header)
{
    [... Your specific code ...]
    return $result;
}
```


```php
function myPreExtractCallBack($p_event, &$p_header)
{
    $info = pathinfo($p_header['filename']);
    // ----- gif files are skipped
    if ($info['extension'] == 'gif') {
      return 0;
    }
    // ----- jpg files are extracted in images folder
    else if ($info['extension'] == 'jpg') {
      $p_header['filename'] = 'images/' . $info['basename'];
      return 1;
    }
    // ----- all other files are simply extracted
    else {
      return 1;
    }
}

$list = $archive->extract(
    PCLZIP_OPT_PATH, 'folder',
    PCLZIP_CB_PRE_EXTRACT, 'myPreExtractCallBack'
);
```




## PCLZIP_CB_POST_EXTRACT（回调函数）

```php

```



```php

```




## PCLZIP_CB_PRE_ADD（回调函数）
```php

```


```php

```




## PCLZIP_CB_POST_ADD（回调函数）

```php

```



```php

```

