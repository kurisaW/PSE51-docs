# imaxdiv

## NAME

imaxdiv — 返回商和余数

## SYNOPSIS

```c
#include <inttypes.h>

imaxdiv_t imaxdiv(intmax_t numer, intmax_t denom);
```

## DESCRIPTION

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`imaxdiv()` 函数应在单个操作中计算 `numer / denom` 和 `numer % denom`。

## RETURN VALUE

`imaxdiv()` 函数应返回一个 `imaxdiv_t` 类型的结构体，包含商和余数两部分。该结构体应包含（顺序不限）成员 `quot`（商）和 `rem`（余数），每个成员的类型均为 `intmax_t`。

如果结果的任何部分无法表示，则行为是未定义的。

## ERRORS

未定义错误。

---

*以下章节为参考信息。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `imaxabs()`
- XBD `<inttypes.h>`

## CHANGE HISTORY

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

---

*参考信息结束。*