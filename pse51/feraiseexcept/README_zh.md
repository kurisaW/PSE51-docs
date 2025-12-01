# feraiseexcept — 引发浮点异常

## 概要

```c
#include <fenv.h>

int feraiseexcept(int excepts);
```

## 描述

`feraiseexcept()` 函数应尝试引发由 `excepts` 参数表示的受支持的浮点异常。引发这些浮点异常的顺序是未指定的，但如果 `excepts` 参数表示 IEC 60559 中针对原子操作的有效并发浮点异常（即溢出和不精确，或下溢和不精确），则应先引发溢出或下溢，再引发不精确异常。`feraiseexcept()` 函数在引发溢出或下溢浮点异常时是否同时引发不精确浮点异常是由实现定义的。

## 返回值

如果参数为零，或者所有指定的异常都已成功引发，`feraiseexcept()` 应返回零。否则，应返回非零值。

## 错误

未定义任何错误。

## 示例

无。

## 应用用法

该效果旨在与算术操作引发的浮点异常类似。因此，由此函数引发的浮点异常的已启用陷阱会被触发。

## 基本原理

允许在引发溢出或下溢时同时引发不精确异常，因为在某些架构上，引发异常的唯一实用方法是执行具有该异常作为副作用的指令。该函数不限于仅接受原子操作的有效并发表达式，因此该函数可用于引发在多个操作中累积的异常。

## 未来方向

无。

## 另请参阅

- `feclearexcept()`
- `fegetexceptflag()`
- `fetestexcept()`
- `<fenv.h>`

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

已纳入 ISO/IEC 9899:1999 标准，技术勘误 1。

### Issue 7

已应用 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0111 [543]。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 The IEEE and The Open Group*