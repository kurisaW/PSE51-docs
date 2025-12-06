# pthread_getconcurrency

## 概要

```c
#include <pthread.h>

int pthread_getconcurrency(void);
```

## 描述

进程中的未绑定线程可能需要也可能不需要同时激活。默认情况下，线程实现确保有足够数量的线程处于激活状态，以便进程能够继续执行。虽然这样做可以节省系统资源，但可能不会产生最有效的并发级别。

`pthread_setconcurrency()` 函数允许应用程序将其期望的并发级别 `new_level` 通知给线程实现。由于此函数调用，实现提供的实际并发级别是未指定的。

如果 `new_level` 为零，则会导致实现自行维护并发级别，就像从未调用过 `pthread_setconcurrency()` 一样。

`pthread_getconcurrency()` 函数应返回先前调用 `pthread_setconcurrency()` 函数所设置的值。如果先前没有调用过 `pthread_setconcurrency()` 函数，此函数应返回零，表示实现正在维护并发级别。

调用 `pthread_setconcurrency()` 应通知实现其期望的并发级别。实现应将此作为提示，而不是要求。

## 返回值

`pthread_getconcurrency()` 函数应始终返回先前调用 `pthread_setconcurrency()` 所设置的并发级别。如果从未调用过 `pthread_setconcurrency()` 函数，`pthread_getconcurrency()` 应返回零。

## 错误

`pthread_getconcurrency()` 未定义任何错误。

## 示例

### 示例 1：获取当前并发级别

```c
#include <stdio.h>
#include <pthread.h>

int main(void) {
    int level;

    level = pthread_getconcurrency();
    printf("Current concurrency level: %d\n", level);

    if (level == 0) {
        printf("Implementation is maintaining concurrency level\n");
    }

    return 0;
}
```

### 示例 2：设置和获取并发级别

```c
#include <stdio.h>
#include <pthread.h>

int main(void) {
    int ret, level;

    // Set desired concurrency level
    ret = pthread_setconcurrency(4);
    if (ret != 0) {
        printf("Failed to set concurrency level\n");
        return 1;
    }

    // Get current concurrency level
    level = pthread_getconcurrency();
    printf("Concurrency level set to: %d\n", level);

    // Reset to implementation-discretion
    ret = pthread_setconcurrency(0);
    if (ret == 0) {
        level = pthread_getconcurrency();
        printf("After reset, concurrency level: %d\n", level);
    }

    return 0;
}
```

## 应用程序使用

应用程序应使用 `pthread_getconcurrency()` 来确定先前使用 `pthread_setconcurrency()` 设置的当前并发级别。该函数特别适用于：

- 检查是否设置了特定的并发级别
- 验证先前 `pthread_setconcurrency()` 调用的效果
- 调试和监视线程并发行为

## 基本原理

如果实现不支持在多个内核调度实体之上多路复用用户线程，则提供 `pthread_setconcurrency()` 和 `pthread_getconcurrency()` 函数是为了源代码兼容性，但在调用时它们应不起作用。为了保持函数语义，当调用 `pthread_setconcurrency()` 时保存 `new_level` 参数，以便后续调用 `pthread_getconcurrency()` 应返回相同的值。

## 未来方向

无。

## 另请参见

- `pthread_setconcurrency()`
- `pthread_create()`
- `pthread_attr_init()`

## 版权

本文部分内容摘录并转载自 IEEE Std 1003.1, 2013 Edition, Standard for Information Technology -- Portable Operating System Interface (POSIX), The Open Group Base Specifications Issue 7, Copyright (C) 2013 by the Institute of Electrical and Electronics Engineers, Inc and The Open Group. 如果此版本与原始 IEEE 和 The Open Group 标准之间存在任何差异，应以原始 IEEE 和 The Open Group 标准为准。原始标准可在线获取：http://www.opengroup.org/unix/online.html。

本页面中出现的任何印刷或格式错误很可能是在将源文件转换为手册页格式时引入的。要报告此类错误，请参见 https://www.kernel.org/doc/man-pages/reporting_bugs.html 。
