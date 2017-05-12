
# 更新日志
英文原文：http://www.phpconcept.net/pclzip/release-notes



## v2.8.2
- 解压文件内容到字符串时`PCLZIP_OPT_EXTRACT_AS_STRING`
支持`PCLZIP_CB_PRE_EXTRACT`、`PCLZIP_CB_POST_EXTRACT`回调函数，并允许在回调函数中修改解压出来的字符串。
- 已知问题修复：
  - `PCLZIP_OPT_REMOVE_ALL_PATH`运行不正确问题
  - 删除`eval()`代码，并直接调用回调函数
  - 正确支持 64位（感谢 WordPress 团队）






## v2.8.1
- 将`PCLZIP_OPT_BY_EREG `参数功能修改为与`PCLZIP_OPT_BY_PREG`一致。
这是因为从 PHP5.3 开始`ereg()`函数将过期。
当你使用`PCLZIP_OPT_BY_EREG`时，PclZip 会自动替换为`PCLZIP_OPT_BY_PREG`。





## v2.8
- 改善解压大文件的情况，增加了使用临时文件的参数。
本功能与 v2.7 中的一致。参数包括
  - PCLZIP_OPT_TEMP_FILE_ON
  - PCLZIP_OPT_TEMP_FILE_OFF
  - PCLZIP_OPT_TEMP_FILE_THRESHOLD
- 增加一个比例常量`PCLZIP_TEMPORARY_FILE_RATIO `，
用于配置自动检测是否使用临时文件的阈值。
- 已知问题修复：
  - 删除返回的文件数组中的路径前缀`.//`





## v2.7
- 改善创建压缩包时包含大文件的情况：
  - PclZip 在碰到大文件时，能自动根据 PHP 内存设置来决定是否使用临时文件
  - `PCLZIP_OPT_ADD_TEMP_FILE_ON`强制所有文件不管大小全部使用临时文件 
  - `PCLZIP_OPT_ADD_TEMP_FILE_OFF`则一律不使用临时文件
  - `PCLZIP_OPT_ADD_TEMP_FILE_THRESHOLD`可设置文件大小阈值来决定是否使用临时文件
  - 使用临时文件会比使用内存的耗时较多，下面是`我`笔记本上的测试：
    - 待压缩的文件大小 88MB
    - 使用内存：18s（max_execution_time=30, memory_limit=180MB）
    - 使用临时文件：23s（max_execution_time=30, memory_limit=30MB）
- 使用` time()`替换`mktime()`，用于限制`E_STRICT`错误信息
- 已知问题修复：
  - Windows 下添加文件到压缩包时，若文件路径不是默认盘符下添加不成功的问题





## v2.6
- 代码优化
- 增加：`PCLZIP_ATT_FILE_COMMENT`参数，用于为单个文件添加注释（但不知有什么用处）
- 增加：`PCLZIP_ATT_FILE_CONTENT`参数，用于将一个字符串直接作为文件添加到压缩包中
- 增加：`PCLZIP_ATT_FILE_MTIME`参数，用于压缩包中文件的日期
- 修复：若压缩包中的文件时间是`0h0m0s`，解压出来会变成当前时间的问题
- 增加：每次操作后返回的文件列表信息中包含`CRC`信息
- 增加：`closedir()`语句
- 修复：当添加目录到压缩包中，并删除其路径时，此目录里面的文件会不正确地多出了一个`/`的问题。
因为在 Linux 中`/`表示根目录的意思
- 增加：在常量定义前增加了判断语句。这允许用户重新定义常量时不需要修改 PclZip 源文件，
在后续升级 PclZip 时能更平滑。




## v2.5
- 增加：允许在添加文件/目录到压缩包时附加各自的属性信息
  - 修改文件名
  - 为每个文件分别指定
  - 修改全名（full name）
  - 修改短名（short name）
  - 兼容全局参数
- 增加：参数： 
  - PCLZIP_ATT_FILE_NAME
  - PCLZIP_ATT_FILE_NEW_SHORT_NAME
  - PCLZIP_ATT_FILE_NEW_FULL_NAME
- 增加：错误编号：`PCLZIP_ERR_INVALID_ATTRIBUTE_VALUE`
- 增加：安全控制参数`PCLZIP_OPT_EXTRACT_DIR_RESTRICTION`，用于限制解压时允许解压到的目录。
因为有时恶意用户可能会通过上传一个 zip 包，由于之前 PclZip 允许解压文件到系统任意位置，
解压后替换掉系统的某些关键文件，从而造成安全问题。
- 增加：`PCLZIP_OPT_EXTRACT_DIR_RESTRICTION`参数，用于限制允许解压的目录。
- 增加：错误编号：`PCLZIP_ERR_DIRECTORY_RESTRICTION`
- 修改：`PclZipUtilPathInclusion()`函数中以`./`开头的目录和路径会在其前面添加当前路径（`getcwd()`）




## v2.4
- 代码优化：删除一些无用的`pack()`调用，以加快代码运行速度
- 修复：`delete()`调用时应无需参数。在 v2.3 中需要参数，在 v2.4 时不再需要。
- 修复：`path_inclusion`函数中，当路径包含多个`../../`时会导致错误。
- 增加：对`magic_quotes_runtime`配置参数的检查。若开启，当正确运行时，PclZip 会禁用它，并重设会原来的值。
这样可解决很多格式不正确的压缩错误。
- 修复：当压缩前的文件大小与压缩后一样时不能正确解压的问题
- 修复：当设置了`PCLZIP_OPT_REMOVE_ALL_PATH`参数后，不再新建目录的问题
- 代码优化：正确关闭`opendir()`打开的东西，这在循环中能更好处理`.`和`..`。



## v2.3
- 修复：当赋值`0xFE49FFE0 `给一个变量时，PHP4 与 PHP5 运行结果不一致的问题




## v2.2
- 尝试：开发新参数`PCLZIP_OPT_CRYPT`，但停止开发了。原因：
在加密或解密时，我需要对两个长整数（比`long`还要长）进行相乘，
但 PHP 并不支持此功能，即使使用了`bcmath`的函数也没用。
我现在还没找到解决方案。。。

- 修复：目录最后忘了添加`/`的问题

- 增加：检查文件是否已被加密。可返回状态`unsupported_encryption（不支持的加密方式）`及
错误编号：PCLZIP_ERR_UNSUPPORTED_ENCRYPTION

- 修复：本地文件头信息中的不正确字段`version need to extract`

- 增加：private 方法`privCheckFileHeaders()`，用于检查本地和中心文件的头信息。
PclZip 现在支持`purpose bit flag bit 3`（译注：参考 [zip 格式规范](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)）
`purpose bit flag bit 3`允许用户在本地文件头信息中不包含这些字段：大小、压缩后大小、crc。

- 增加：`PCLZIP_ERR_UNSUPPORTED_COMPRESSION`错误编码，用于表示检测到 PclZip 不支持的的压缩算法。
PclZip 仅支持`（deflate）`算法。在 v2.2 之前，PclZip 不会检测压缩包中的压缩算法，
在 v2.2 及之后会进行压缩算法检测，若发现不支持的压缩算法，会返回`unsupported_compression`状态和
`PCLZIP_ERR_UNSUPPORTED_COMPRESSION`错误码。

- 增加：`PCLZIP_OPT_STOP_ON_ERROR`参数，用于在解压时碰到第一个解压错误时就立即停止后续所有的解压
（默认情况下，PclZip 解压碰到错误时仅在返回文件列表属性值标记错误状态，并继续进行后续其他文件的解压）。
可能的错误包括：
  - 已存在同名目录
  - 已存在同名的且时间更新的文件
  - 已存在受保护的文件
  
- 增加：`PCLZIP_OPT_REPLACE_NEWER`参数。默认情况下，PclZip 解压时遇到文件系统已存在同名文件，
但文件系统中的文件时间更老的话，PclZip 会自动用解压出来的文件替换掉文件系统的老文件；
若文件系统的文件时间更新，PclZip 不会替换该文件。本参数用于强制替换文件系统的已存在文件，不管时间新旧。

- 改善：`PclZipUtilOption()`函数
- 增加：支持在压缩包最后附带尾部字节。在 v2.2 之前，PclZip 会检测中心目录应在压缩包的最后位置。
Crypt 在加密或解密 zip 包时，会在解密完毕时在文件最后添加`0 bytes`。PclZip 现在支持此功能。




## v2.1
- 增加：`PCLZIP_OPT_EXTRACT_IN_OUTPUT `参数，用于直接解压文件输出到标准输出中
- 增加：文件注释描述参数
  - PCLZIP_OPT_COMMENT：创建文件注释，若已存在注释，则替换原有注释
  - PCLZIP_OPT_ADD_COMMENT：创建文件注释，若已存在注释，则追加到已有注释后面
  - PCLZIP_OPT_PREPEND_COMMENT：在已有文件注释前增加注释
- 增加：在解压过程的回调函数中终止后续解压的功能。只需在回调函数中返回`2`即可停止后续解压。
对于解压前的回调函数，会在解压当前文件前停止；对于解压后的回调函数，会在后续文件停止。
- 增加：合并两个压缩包时，会自动合并注释，并以空格分隔。
- 修复：删除压缩包内所有文件时，并没删除的问题。
- 修复：若文件名就叫`0`，会导致 PclZip 创建压缩包或添加属性时意外终止。




## v2.0
注意：v2.0 与 v1.x 存在不兼容，你若从 v1.x 升级到 v2.0，你需要修改你原有的代码，请认真往下看。
- 增加：可按压缩包内的索引、名字、正则表达式来删除压缩包中的文件/目录。
对应到方法是`delete()`，支持下面参数：
  - PCLZIP_OPT_BY_INDEX
  - PCLZIP_OPT_BY_NAME
  - PCLZIP_OPT_BY_EREG 
  - PCLZIP_OPT_BY_PREG
  
- 增加：支持按正则表达式解压压缩包内的文件/目录。
使用方式是：`extract()`+`PCLZIP_OPT_BY_EREG（或 PCLZIP_OPT_BY_PREG ）`+正则表达式。

- 增加：支持使用`extract()`方法按压缩包内的文件索引来解压文件。这是`extractByIndex()`改良版。

- 增加：支持按名字解压压缩包内的文件/目录。对应方法是：
  - 方法：extract()
  - 参数：PCLZIP_OPT_BY_NAME
  - 参数值：一个文件名字符串或，一个文件名列表数组
  - 若想解压某个目录的全部内容，应在文件名参数的最后带上`/`

- 增加：`PCLZIP_OPT_NO_COMPRESSION`参数，用于不压缩直接添加文件到压缩包中。

- 增加：`PCLZIP_OPT_EXTRACT_AS_STRING`参数，用于直接解压文件到字符串中，而非解压到文件系统或临时文件中。

- 增加：`PCLZIP_SEPARATOR`常量，用于设置单字符串中表示多个文件名时的分隔符，默认值是英文逗号`,`，
而不是以前版本中说的空格` `。（注意：此常量的值与 v1.x 的不兼容！）

- 优化：通过不使用临时文件（译注：改用内存），提高解压或压缩时的性能。

- 增加：对空文件名的检测，这在被删除的路径与被 zip 压缩的目录是一样的时候会有用。
目录本身不会被 zip 压缩（['status'] = filtered），压缩的是目录中的内容。

- 优化：对 Windows 路径的更好支持（感谢 manus@manusfreedom.com 的帮助）

- 修复：压缩包已存在，且大小为 0 时，`add()`方法会出错的问题。

- 修改：删除`OS_WINDOWS`常量，改用`php_uname()`函数

- 增加：解压文件时，支持指定索引的范围
 
- 优化：修改内部的目录管理（更好的支持内部标记）




## v1.3.1
- 修复：解压过程中，执行回调函数时的相关问题




## v1.3
- 删除：重复的`include`检查，现在使用的是`include_once()`、`require_once()`。

- 修改：错误处理机制：
  - 删除对外部错误库处理的使用
  - 之前的`PclError()`函数被修改为内部的等价的函数
  - 若修改`PCLZIP_ERROR_EXTERNAL`，你依然可使用之前的库
  - 引入错误标号常量，避免之前的纯整数看不懂，这会极大方便后续的维护
  - 引入对应的错误处理函数：`errorCode()`、`errorName()`、`errorInfo()`
  
- 删除：废弃函数调用中直接传引用

- 添加：为`extract()`、`extractByIndex()`、`create()`、`add()`
方法的调用增加变量参数，而非固定的参数数量。

- 增加：`PCLZIP_OPT_REMOVE_ALL_PATH`参数，用于在解压或压缩时去除所有的文件路径，无需指定任何路径值。
支持的方法：
  - extract()
  - extractByIndex()
  - create()
  - add()

- 增加：`PCLZIP_OPT_SET_CHMOD`参数，用于在解压完毕后，对解压出来的文件进行权限修改（基于 PHP `chmod()`）。
支持的方法：
  - extract()
  - extractByIndex()
  
- 增加：回调函数机制。
使用回调函数，你可进行一些相关的操作。
支持回调函数的方法：
  - add()
  - extract()
  - extractByIndex()
  - create()
  - 回调函数参数包括：
    - PCLZIP_CB_PRE_EXTRACT：在每个文件解压出来前被调用。用户可以在修改文件名、跳过不解压当前文件
    （会被标记为`skipped`）等。
    - PCLZIP_CB_POST_EXTRACT：在每个文件解压出来后被调用。这里没有什么可以被修改的。
    - PCLZIP_CB_PRE_ADD：在每个文件被添加到压缩包前被调用。用户可以在修改文件名、跳过不添加当前文件
     （会被标记为`skipped`）等。
    - PCLZIP_CB_POST_ADD：在每个文件被添加到压缩包后被调用。这里没有什么可以被修改的。

- 增加：会增加两个状态标记到返回的文件属性列表中：
  - skipped：回调函数中主动设置为跳过时
  - filename_too_long：文件名太长时（此时文件不会被添加到压缩包）

- 增加：` PclZipUtilPathInclusion()`函数，用于检查目录的路径。

- 增加：在某些操作前，检查压缩包中的文件是否已存在（如 list 操作）

- 增加：初始化头部数组中的字段`index`。当操作方法没有明确设置此字段值时，默认为`-1`。



## v1.2
- 增加：复制方法（`duplicate()`）
- 增加：合并方法（`merge()`），用于将另一个压缩包内的所有内容合并到当前对象的压缩包中。
合并过程中，并不会进行任何检查，如不会检查同名的文件。
- 优化：对中心目录结尾的搜索



## v1.1.2
- 修改：PclZip 的使用协议，现在 PclZip 使用的协议是[GNU / LGPL license](http://www.phpconcept.net/pclzip/gnu-lgpl.txt)
  > 译注：上面链接已失效
- 增加：`PCLZIP_TEMPORARY_DIR`常量，用于指定 PclZip 使用的临时文件夹。
- 完善：`rename()`函数，此函数在不同的文件系统中可能会出错，将使用`copy()`+`unlink()`替代。
- 修复：WinZip 不能对 PclZip 生成的压缩包内的文件进行删除或增加的问题。



## v1.1.1
- 修复：不做任何压缩直接添加文件到压缩包后，不能成功解压的问题



## v1.1
- 增加：`PclZip::Add()`方法
- 增加：`PclZip::ExtractByIndex()`方法 
- 增加：`PclZip::DeleteByIndex()`方法 
- 修复：在某些场景下，使用绝对路径压缩文件时出错的问题



## v1.0.1
- 修复：当文件大于`PCLZIP_READ_BLOCK_SIZE`（默认：1024 字节）设置的值时会压缩出错的问题
