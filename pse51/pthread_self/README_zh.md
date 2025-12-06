# pthread_self — 获取调用线程ID

## 概要

```c
#include <pthread.h>

pthread_t pthread_self(void);
```

## 描述

`pthread_self()` 函数应返回调用线程的线程ID。

## 返回值

`pthread_self()` 函数应总是成功的，没有保留任何返回值来表示错误。

## 错误

未定义错误。

*以下章节为参考信息。*

## 示例

无。

## 应用用法

无。

## 基本原理

`pthread_self()` 函数提供了类似于进程的 `getpid()` 函数的功能，其基本原理相同：创建调用不会向被创建的线程提供线程ID。

## 未来方向

无。

## 参见

- `pthread_create()`
- `pthread_equal()`
- `<pthread.h>`

## 变更历史

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含在内。

### Issue 6

`pthread_self()` 函数被标记为 Threads（线程）选项的一部分。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #063，更新了返回值部分。

`pthread_self()` 函数从 Threads（线程）选项移动到 Base（基础）。

