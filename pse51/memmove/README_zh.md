# memmove

## SYNOPSIS

```c
#include <string.h>

void *memmove(void *s1, const void *s2, size_t n);
```

## DESCRIPTION

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 遵从 ISO C 标准。

`memmove()` 函数应将 `n` 个字节从 `s2` 指向的对象复制到 `s1` 指向的对象中。复制过程的行为如同：先将 `s2` 指向的对象中的 `n` 个字节复制到一个与 `s1` 和 `s2` 指向的对象都不重叠的、包含 `n` 个字节的临时数组中，然后再将临时数组中的 `n` 个字节复制到 `s1` 指向的对象中。

[CX] `memmove()` 函数在有效输入时不应更改 `errno` 的设置。

## RETURN VALUE

`memmove()` 函数应返回 `s1`；没有保留返回值来指示错误。

## ERRORS

未定义错误。

---

*以下各节为资料性内容。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

XBD `<string.h>`

## CHANGE HISTORY

首次发布于 Issue 4。源自 ANSI C 标准。

### Issue 8

应用了 Austin Group 缺陷 448，添加了 `memmove()` 在有效输入时不应更改 `errno` 设置的要求。

---
