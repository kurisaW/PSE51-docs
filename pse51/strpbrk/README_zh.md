# strpbrk

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

strpbrk — 扫描字符串中的字节

## SYNOPSIS

```c
#include <string.h>

char *strpbrk(const char *s1, const char *s2);
```

## DESCRIPTION

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`strpbrk()` 函数应定位 `s1` 所指向的字符串中第一次出现的 `s2` 所指向字符串中的任何字节。

[CX] `strpbrk()` 函数在有效输入上不应改变 `errno` 的设置。

## RETURN VALUE

成功完成后，`strpbrk()` 应返回指向该字节的指针；如果 `s2` 中的任何字节在 `s1` 中都没有出现，则返回空指针。

## ERRORS

未定义错误。

---

*以下部分为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`strchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strchr.html)
- [`strrchr()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strrchr.html)
- XBD [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 8

应用了 Austin Group Defect 448，增加了 `strpbrk()` 在有效输入上不改变 `errno` 设置的要求。

