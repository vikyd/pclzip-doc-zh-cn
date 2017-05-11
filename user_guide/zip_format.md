
# zip 压缩格式
PclZip 支持的压缩格式是 PKZIP，PKZIP 格式的详细信息见：http://www.phpconcept.net/pclzip/pkzip.txt。

译注：
- PKZIP 是一个支持 zip 压缩格式的软件
- 估计 PclZip 作者想表达的就是通用的 zip 格式而已，而非 PKZIP 特有的格式。
- 实际使用起来，PclZip 与 PKZIP 并无关系
- 上面的 PKZIP 链接已失效（404）

下面是 zip 压缩包格式的概述：

1. 压缩包内的每个文件/目录，都有一个头信息，里面存储了文件的文件名、文件大小等信息。
数据压缩采用的是 GZIP 算法，详见：http://www.phpconcept.net/pclzip/rfc1952-gzip.txt。

译注：原文的 GZIP 链接已失效（404）


2. 在压缩包的最后位置专门存储压缩包内每个文件的属性信息和整个压缩包的属性信息。
这样的好处是：无需解释整个压缩包就可快速获得压缩包的概要信息。

3. PKZIP 格式定义了很多强大的属性，但 PclZip 至今仍不支持。
此外，PclZip 不支持由多个磁盘压缩组成的 zip 包。

译注：
- 估计 PKZIP 中很多属性不是 zip 规范的。
- 不太理解 `PclZip 不支持由多个磁盘压缩组成的 zip 包` 的意思，不过一般也不会碰到此问题。

