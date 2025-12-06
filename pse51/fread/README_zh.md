# fread

## SYNOPSIS

```c
#include <stdio.h>

size_t fread(void *restrict ptr, size_t size, size_t nitems,
             FILE *restrict stream);
```

## DESCRIPTION

`fread()` 函数应从 `stream` 所指向的流中读取最多 `nitems` 个元素到 `ptr` 所指向的数组，每个元素的大小由 `size` 字节指定。对于每个对象，应调用 `size` 次 `fgetc()` 函数，并将结果按读取顺序存储在一个完全覆盖该对象的 **unsigned char** 数组中。流的文件位置指示器（如果已定义）应按成功读取的字节数前进。如果发生错误，流的文件位置指示器的结果值是未指定的。如果读取了部分元素，其值是未指定的。

`fread()` 函数可能会标记与 `stream` 关联的文件的最后数据访问时间戳以供更新。最后一次数据访问时间戳应通过第一次成功执行使用 `stream` 的 `fgetc()`、`fgets()`、`fread()`、`fscanf()`、`getc()`、`getchar()`、`getdelim()`、`getline()` 或 `scanf()` 函数来标记更新，这些函数返回的数据不是由先前对 `ungetc()` 的调用提供的。

## RETURN VALUE

`fread()` 函数应返回成功读取的元素数量，该数量仅当遇到错误或文件结束条件或 `size` 为零时才小于 `nitems`。如果 `size` 或 `nitems` 为 0，`fread()` 应返回 0，且数组的内容和流的状态应保持不变。否则，如果发生错误，应设置流的错误指示器，并应设置 `errno` 以指示错误。

## ERRORS

参考 `fgetc()`。

*以下章节为参考信息。*

## EXAMPLES

### 从流中读取

以下示例将单个100字节定长记录从 `fp` 流传输到 `buf` 所指向的数组。

```c
#include <stdio.h>
...
size_t elements_read;
char buf[100];
FILE *fp;
...
elements_read = fread(buf, sizeof(buf), 1, fp);
...
```

如果发生读取错误，`elements_read` 将为零，但从流中读取的字节数可能从零到 `sizeof(buf)`-1 之间的任何值。

以下示例从 `fp` 流中读取多个单字节元素到 `buf` 所指向的数组。

```c
#include <stdio.h>
...
size_t bytes_read;
char buf[100];
FILE *fp;
...
bytes_read = fread(buf, 1, sizeof(buf), fp);
...
```

如果发生读取错误，`bytes_read` 将包含从流中读取的字节数。

## APPLICATION USAGE

必须使用 `ferror()` 或 `feof()` 函数来区分错误条件和文件结束条件。

由于元素长度和字节顺序可能存在差异，使用 `fwrite()` 写入的文件是应用程序相关的，可能无法被不同的应用程序或同一应用程序在不同处理器上使用 `fread()` 读取。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [2.5 标准I/O流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feof.html)
- [`ferror()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ferror.html)
- [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html)
- [`fopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)
- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

为了与 ISO/IEC 9899:1999 标准对齐，进行了以下更改：

- 更新了 `fread()` 原型。
- 更新了 DESCRIPTION 以描述如何存储来自 `fgetc()` 调用的字节。

### Issue 7

进行了与支持细粒度时间戳相关的更改。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0178 [232] 和 XSH/TC1-2008/0179 [14]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0129 [926]。

### Issue 8

应用了 Austin Group Defect 1196，阐明了 RETURN VALUE 部分。

应用了 Austin Group Defect 1330，删除了过时的接口。

应用了 Austin Group Defect 1624，更改了 RETURN VALUE 部分。

---

