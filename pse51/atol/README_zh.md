# atol, atoll — 将字符串转换为长整型

## 概要

```c
#include <stdlib.h>

long atol(const char *nptr);
long long atoll(const char *nptr);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

除下述说明外，`atol(nptr)` 调用应等价于：

```c
strtol(nptr, (char **)NULL, 10)
```

除下述说明外，`atoll(nptr)` 调用应等价于：

```c
strtoll(nptr, (char **)NULL, 10)
```

错误处理方式可能有所不同。如果值无法表示，则行为未定义。

## 返回值

如果值可以被表示，这些函数应返回转换后的值。

## 错误

未定义错误。

---

*以下章节为参考信息。*

## 示例

无。

## 应用程序用法

如果不知道数值是否在范围内，应使用 `strtol()` 或 `strtoll()`，因为 `atol()` 和 `atoll()` 不需要执行任何错误检查。

## 原理

无。

## 未来方向

无。

## 参见

- `strtol()`
- `<stdlib.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

添加了 `atoll()` 函数以与 ISO/IEC 9899:1999 标准保持一致。

### Issue 7

应用了 SD5-XSH-ERN-61，更正了 `atoll()` 的描述。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0046 [892]。

---

