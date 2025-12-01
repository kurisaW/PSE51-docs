# pthread_equal — 比较线程 ID

## 概要

```c
#include <pthread.h>

int pthread_equal(pthread_t t1, pthread_t t2);
```

## 描述

此函数用于比较线程 ID `t1` 和 `t2`。

## 返回值

如果 `t1` 和 `t2` 相等，`pthread_equal()` 函数应返回非零值；否则，应返回零。

如果 `t1` 或 `t2` ���是有效的线程 ID 且不等于 PTHREAD_NULL，则行为未定义。

## 错误

未定义任何错误。

`pthread_equal()` 函数不应返回 [EINTR] 错误码。

*以下章节为参考信息。*

## 示例

无。

## 应用程序使用

无。

## 基本原理

实现可以选择将线程 ID 定义为结构体。这比使用 `int` 类型提供了额外的灵活性和健壮性。例如，线程 ID 可以包含序列号，允许检测"悬空 ID"（已分离线程 ID 的副本）。由于 C 语言不支持结构体类型的比较，因此提供了 `pthread_equal()` 函数来比较线程 ID。

## 未来方向

无。

## 参见

- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- [`pthread_self()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_self.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史

**首次发布于 Issue 5。** 为与 POSIX 线程扩展对齐而包含。

**Issue 6**
- `pthread_equal()` 函数被标记为 Threads 选项的一部分。

**Issue 7**
- `pthread_equal()` 函数从 Threads 选项移动到 Base。

**Issue 8**
- 应用了 Austin Group Defect 599，更改了 RETURN VALUE 章节以提及 PTHREAD_NULL。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*