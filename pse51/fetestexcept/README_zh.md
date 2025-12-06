# fetestexcept — 测试浮点异常标志

## 概要

```c
#include <fenv.h>

int fetestexcept(int excepts);
```

## 描述

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`fetestexcept()` ��数应确定指定浮点异常标志子集中当前已设置的标志。`excepts` 参数指定要查询的浮点状态标志。

## 返回值

`fetestexcept()` 函数应返回与 `excepts` 中包含的当前已设置浮点异常相对应的浮点异常宏的按位或运算值。

## 错误

未定义错误。

---

*以下部分为参考信息。*

## 示例

以下示例在设置了无效异常时调用函数 `f()`，然后在设置了溢出异常时调用函数 `g()`：

```c
#include <fenv.h>
/* ... */
{
    #pragma STDC FENV_ACCESS ON
    int set_excepts;
    feclearexcept(FE_INVALID | FE_OVERFLOW);
    // 可能引发异常
    set_excepts = fetestexcept(FE_INVALID | FE_OVERFLOW);
    if (set_excepts & FE_INVALID) f();
    if (set_excepts & FE_OVERFLOW) g();
    /* ... */
}
```

## 应用程序用法

无。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- [`feclearexcept()`](feclearexcept.md)
- [`fegetexceptflag()`](fegetexceptflag.md)
- [`feraiseexcept()`](feraiseexcept.md)

XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

