# lldiv

## SYNOPSIS

```c
#include <stdlib.h>

ldiv_t ldiv(long numer, long denom);
lldiv_t lldiv(long long numer, long long denom);
```

## DESCRIPTION

这些函数应计算分子 `numer` 除以分母 `denom` 的商和余数。如果除法不精确，结果商是幅度较小的最接近代数商的 **long** 整数（对于 `ldiv()` 函数）或 **long long** 整数（对于 `lldiv()` 函数）。如果结果无法表示，行为是未定义的；否则，`quot` * `denom` + `rem` 应等于 `numer`。

## RETURN VALUE

`ldiv()` 函数应返回一个 **ldiv_t** 类型的结构体，包含商和余数。该结构体应按任意顺序包含以下成员：

```c
long   quot;    /* 商 */
long   rem;     /* 余数 */
```

`lldiv()` 函数应返回一个 **lldiv_t** 类型的结构体，包含商和余数。该结构体应按任意顺序包含以下成员：

```c
long long   quot;    /* 商 */
long long   rem;     /* 余数 */
```

## ERRORS

未定义错误。

* * *

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`div()`](div.md)

[XBD `<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## CHANGE HISTORY

首次在 Issue 4 中发布。派生自 ISO C 标准。

### Issue 6

为与 ISO/IEC 9899:1999 标准保持一致，添加了 `lldiv()` 函数。

---
