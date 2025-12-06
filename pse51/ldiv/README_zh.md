# ldiv, lldiv — 计算长整型除法的商和余数

## 概要

```c
#include <stdlib.h>

ldiv_t ldiv(long numer, long denom);
lldiv_t lldiv(long long numer, long long denom);
```

## 描述

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

这些函数应计算分子 `numer` 除以分母 `denom` 的商和余数。如果除法不精确，结果商是幅度更小且最接近代数商的 **long** 整数（对于 `ldiv()` 函数）或 **long long** 整数（对于 `lldiv()` 函数）。如果结果无法表示，则行为未定义；否则，`quot * denom + rem` 应等于 `numer`。

## 返回值

`ldiv()` 函数应返回 **ldiv_t** 类型的结构体，包含商和余数。该结构体应包含以下成员，顺序不���：

```c
long   quot;    /* 商 */
long   rem;     /* 余数 */
```

`lldiv()` 函数应返回 **lldiv_t** 类型的结构体，包含商和余数。该结构体应包含以下成员，顺序不限：

```c
long long   quot;    /* 商 */
long long   rem;     /* 余数 */
```

## 错误

未定义任何错误。

## 参见

- [`div()`](div.html)
- [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## 变更历史

首次发布于 Issue 4。源自 ISO C 标准。

### Issue 6

添加 `lldiv()` 函数以与 ISO/IEC 9899:1999 标准保持一致。

---
