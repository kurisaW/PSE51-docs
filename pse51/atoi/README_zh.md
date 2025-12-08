# atoi — 将字符串转换为整数

## 概要

```c
#include <stdlib.h>

int atoi(const char *str);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非预期的。本卷 POSIX.1-2024 遵从 ISO C 标准。

调用 `atoi(str)` 应等效于：

```c
(int) strtol(str, (char **)NULL, 10)
```

但错误处理可能有所不同。如果值无法表示，则行为未定义。

## 返回值

如果值可以表示，`atoi()` 函数应返回转换后的值。

## 错误

未定义任何错误。

---

*以下章节为参考信息。*

## 示例

### 转换参数

以下示例检查程序的使用是否正确。如果存在参数且此参数的十进制转换（使用 `atoi()` 获得）大于 0，则程序具有有效的等待事件的分钟数。

```c
#include <stdlib.h>
#include <stdio.h>
...
int minutes_to_event;
...
if (argc < 2 || (minutes_to_event = atoi(argv[1])) <= 0) {
   fprintf(stderr, "Usage: %s minutes\n", argv[0]); exit(1);
}
...
```

## 应用程序使用

`atoi()` 函数已被 `strtol()` 包含，但由于在现有代码中广泛使用而保留。如果不知道数字是否在范围内，应使用 `strtol()`，因为 `atoi()` 不需要执行任何错误检查。

## 理由

无。

## 未来方向

无。

## 另请参阅

- `strtol()`
- `<stdlib.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 8

应用了 Austin Group Defect 1541，更改了示例部分。

---
