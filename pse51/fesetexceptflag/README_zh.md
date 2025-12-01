# fegetexceptflag, fesetexceptflag — 获取和设置浮点状态标志

## 概要

```c
#include <fenv.h>

int fegetexceptflag(fexcept_t *flagp, int excepts);
int fesetexceptflag(const fexcept_t *flagp, int excepts);
```

## 描述

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`fegetexceptflag()` 函数应尝试将由参数 `excepts` 指示的浮点状态标志的状态以实现定义的表示形式存储到由参数 `flagp` 指向的对象中。

`fesetexceptflag()` 函数应尝试将由参数 `excepts` 指示的浮点状态标志设置为存储在由参数 `flagp` 指向的对象中的状态。`flagp` 指向的值应由之前对 `fegetexceptflag()` 的调用设置，该调用的第二个参数至少表示了由参数 `excepts` 表示的那些浮点异常。此函数不会引发浮点异常，而只是设置标志的状态。

## 返回值

如果表示形式成功存储，`fegetexceptflag()` 应返回零。否则，应返回非零值。如果 `excepts` 参数为零或所有指定的异常都已成功设置，`fesetexceptflag()` 应返回零。否则，应返回非零值。

## 错误

未定义错误。

## 示例

无。

## 应用用法

无。

## 基本原理

无。

## 未来方向

无。

## 另请参见

- [`feclearexcept()`](feclearexcept.md)
- [`feraiseexcept()`](feraiseexcept.md)
- [`fetestexcept()`](fetestexcept.md)
- XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

ISO/IEC 9899:1999 标准的技术勘误 1 已被纳入。