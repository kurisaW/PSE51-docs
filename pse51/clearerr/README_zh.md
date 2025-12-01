# clearerr

## NAME

clearerr — 清除流的指示器

## SYNOPSIS

```c
#include <stdio.h>

void clearerr(FILE *stream);
```

## DESCRIPTION

本参考页所描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`clearerr()` 函数应清除 `stream` 参数所指向流的文件结束指示器和错误指示器。

如果 `stream` 参数有效，`clearerr()` 函数不应更改 `errno` 的设置。

## RETURN VALUE

`clearerr()` 函数不返回任何值。

## ERRORS

未定义任何错误。

---

*以下部分为提供参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

XBD `<stdio.h>`

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008 Technical Corrigendum 1, XSH/TC1-2008/0057 [401]。

---

*参考信息结束。*