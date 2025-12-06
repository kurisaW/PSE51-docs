# feupdateenv — 更新浮点环境

## 概要

```c
#include <fenv.h>
int feupdateenv(const fenv_t *envp);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`feupdateenv()` 函数应尝试将当前触发的浮点异常保存在其自动存储中，尝试安装由 `envp` 指向的对象所表示的浮点环境，然后尝试触发保存的浮点异常。参数 `envp` 应指向通过调用 `feholdexcept()` 或 `fegetenv()` 设置的对象，或等于一个浮点环境宏。

## 返回值

`feupdateenv()` 函数应返回零值当且仅当所有必需的操作都成功执行。

## 错误

未定义错误。

*以下章节为参考信息。*

## 示例

以下示例展示了隐藏虚假下溢浮点异常的示例代码：

```c
#include <fenv.h>
double f(double x)
{
    #pragma STDC FENV_ACCESS ON
    double result;
    fenv_t save_env;
    feholdexcept(&save_env);
    // 计算结果
    if (/* 测试虚假下溢 */)
    feclearexcept(FE_UNDERFLOW);
    feupdateenv(&save_env);
    return result;
}
```

## 应用用法

无。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`fegetenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetenv.html)
- [`feholdexcept()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feholdexcept.html)
- XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

已纳入 ISO/IEC 9899:1999 标准技术勘误 1。

