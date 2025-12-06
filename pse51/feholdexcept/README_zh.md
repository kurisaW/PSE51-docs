# feholdexcept

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## 名称

feholdexcept — 保存当前浮点环境

## 概要

```c
#include <fenv.h>

int feholdexcept(fenv_t *envp);
```

## 描述

[CX] 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`feholdexcept()` 函数应将当前浮点环境保存在 `envp` 指向的对象中，清除浮点状态标志，然后为所有浮点异常安装非停止（浮点异常时继续运行）模式（如果可用）。

## 返回值

`feholdexcept()` 函数当且仅当成功安装非停止浮点异常处理时返回零。

## 错误

未定义错误。

*以下部分为参考信息。*

## 示例

无。

## 应用程序用法

无。

## 基本原理

`feholdexcept()` 函数在典型的 IEC 60559:1989 标准实现上应该是有效的，这些实现具有默认的非停止模式和至少一种其他模式用于陷阱处理或中止。如果实现只提供非停止模式，那么安装非停止模式就是简单的操作。

## 未来方向

无。

## 另请参阅

- [`fegetenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fegetenv.html)
- [`feupdateenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feupdateenv.html)
- XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

