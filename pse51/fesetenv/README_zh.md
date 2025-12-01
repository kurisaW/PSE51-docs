# fegetenv, fesetenv — 获取和设置当前浮点环境

## 概要

```c
#include <fenv.h>

int fegetenv(fenv_t *envp);
int fesetenv(const fenv_t *envp);
```

## 描述

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`fegetenv()` 函数应尝试将当前浮点环境存储在 `envp` 指向的对象中。

`fesetenv()` 函数应尝试建立由 `envp` 指向的对象所表示的浮点环境。参数 `envp` 应指向通过调用 `fegetenv()` 或 `feholdexcept()` 设置的对象，或者等于一个浮点环境宏。`fesetenv()` 函数不会引发浮点异常，只是安装通过其参数表示的浮点状态标志的状态。

## 返回值

如果表示被成功存储，`fegetenv()` 应返回零。否则，它应返回非零值。如果环境被成功建立，`fesetenv()` 应返回零。否则，它应返回非零值。

## 错误

未定义错误。

## 示例

无。

## 应用用法

无。

## 原理

无。

## 未来方向

无。

## 另请参阅

- `feholdexcept()`
- `feupdateenv()`
- `<fenv.h>`

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

已纳入 ISO/IEC 9899:1999 标准，技术勘误 1。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*