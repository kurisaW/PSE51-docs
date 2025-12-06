# feclearexcept — 清除浮点异常

## 概要

```c
#include <fenv.h>

int feclearexcept(int excepts);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`feclearexcept()` 函数应尝试清除由 `excepts` 表示的受支持的浮点异常。

## 返回值

如果参数为零或所有指定的异常都已成功清除，`feclearexcept()` 应返回零。否则，应返回非零值。

## 错误

未定义错误。

---

*以下部分为参考信息。*

## 示例

无。

## 应用程序用法

无。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- [`fegetexceptflag()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetexceptflag.html)
- [`feraiseexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feraiseexcept.html)
- [`fetestexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fetestexcept.html)

XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

已纳入 ISO/IEC 9899:1999 标准，技术勘误 1。

