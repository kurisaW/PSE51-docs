# getchar

## SYNOPSIS

```c
#include <stdio.h>

int getchar(void);
```

## DESCRIPTION

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 这卷文档遵循 ISO C 标准。

`getchar()` 函数应等价于 `getc(stdin)`。

## RETURN VALUE

参考 [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html)。

## ERRORS

参考 [`fgetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgetc.html)。

---

*以下为提供信息的章节。*

## EXAMPLES

无。

## APPLICATION USAGE

如果将 `getchar()` 返回的整数值存储到 `char` 类型的变量中，然后与整数常量 EOF 进行比较，这个比较可能永远不会成功，因为 `char` 类型变量在扩展为整数时的符号扩展是由实现定义的。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [2.5 标准I/O流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008 Technical Corrigendum 1, XSH/TC1-2008/0236 [14]。

---
