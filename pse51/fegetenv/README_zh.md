# fegetenv, fesetenv — 获取和设置当前浮点环境

## 概要

```c
#include <fenv.h>

int fegetenv(fenv_t *envp);
int fesetenv(const fenv_t *envp);
```

## 描述

本参考页描述的功能与 ISO C 标准保持一致。如果此处描述的要求与 ISO C 标准之间存在冲突，则属无心之失。本卷 POSIX.1-2024 遵从 ISO C 标准。

`fegetenv()` 函数应尝试将当前浮点环境存储到 `envp` 指向的对象中。

`fesetenv()` 函数应尝试建立由 `envp` 指向的对象所表示的浮点环境。参数 `envp` 应指向通过调用 `fegetenv()` 或 `feholdexcept()` 设置的对象，或等于一个浮点环境宏。`fesetenv()` 函数不会引发浮点异常，只安装通过其参数表示的浮点状态标志的状态。

## 返回值

如果表示已成功存储，`fegetenv()` 应返回零。否则，应返回非零值。如果环境已成功建立，`fesetenv()` 应返回零。否则，应返回非零值。

## 错误

未定义任何错误。

## 示例

无。

## 应用程序用法

无。

## 基本原理

无。

## 未来方向

无。

## 另请参阅

- [`feholdexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feholdexcept.html)
- [`feupdateenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feupdateenv.html)
- [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 更改历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

已纳入 ISO/IEC 9899:1999 标准，技术勘误 1。

---
