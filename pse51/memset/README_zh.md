# memset

## SYNOPSIS

```c
#include <string.h>

void *memset(void *s, int c, size_t n);
```

## DESCRIPTION

[选项开始] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵从 ISO C 标准。[选项结束]

`memset()` 函数应将 `c`（转换为 **unsigned char** 类型）复制到由 `s` 指向的对象的前 `n` 个字节中的每一个字节。

[选项开始] `memset()` 函数在有效输入时不应更改 errno 的设置。[选项结束]

## RETURN VALUE

`memset()` 函数应返回 `s`；没有保留用于指示错误的返回值。

## ERRORS

未定义错误。

* * *

_以下章节为信息性内容。_

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

XBD [\<string.h\>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 8

应用了 Austin Group Defect 448，增加了 `memset()` 在有效输入时不更改 errno 设置的要求。

_信息性文本结束。_

* * *
