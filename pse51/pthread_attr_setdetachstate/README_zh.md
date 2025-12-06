# pthread_attr_getdetachstate, pthread_attr_setdetachstate — 获取和设置分离状态属性

## 概要

```c
#include <pthread.h>

int pthread_attr_getdetachstate(const pthread_attr_t *attr, int *detachstate);
int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate);
```

## 描述

`detachstate` 属性控制线程是否以分离状态创建。如果线程以分离状态创建，那么使用 `pthread_detach()` 或 `pthread_join()` 函数操作新创建线程的 ID 是一个错误。

`pthread_attr_getdetachstate()` 和 `pthread_attr_setdetachstate()` 函数分别用于获取和设置 `attr` 对象中的 `detachstate` 属性。

对于 `pthread_attr_getdetachstate()`，`detachstate` 应被设置为 `PTHREAD_CREATE_DETACHED` 或 `PTHREAD_CREATE_JOINABLE`。

对于 `pthread_attr_setdetachstate()`，应用程序应将 `detachstate` 设置为 `PTHREAD_CREATE_DETACHED` 或 `PTHREAD_CREATE_JOINABLE`。

值为 `PTHREAD_CREATE_DETACHED` 将导致所有使用 `attr` 创建的线程处于分离状态，而使用值为 `PTHREAD_CREATE_JOINABLE` 将导致所有使用 `attr` 创建的线程处于可连接状态。`detachstate` 属性的默认值为 `PTHREAD_CREATE_JOINABLE`。

如果传递给 `pthread_attr_getdetachstate()` 或 `pthread_attr_setdetachstate()` 的 `attr` 参数值不引用已初始化的线程属性对象，则行为未定义。

## 返回值

成功完成时，`pthread_attr_getdetachstate()` 和 `pthread_attr_setdetachstate()` 应返回值 0；否则，应返回错误编号以指示错误。

如果成功，`pthread_attr_getdetachstate()` 函数将 `detachstate` 属性的值存储在 `detachstate` 中。

## 错误

`pthread_attr_setdetachstate()` 函数应在以下情况失败：

- **[EINVAL]**
  - `detachstate` 的值无效

这些函数不应返回 [EINTR] 错误代码。

## 示例

### 获取分离状态属性

本示例演示如何获取线程属性对象的 `detachstate` 属性。

```c
#include <pthread.h>

pthread_attr_t thread_attr;
int            detachstate;
int            rc;

/* 初始化 thread_attr 的代码 */
...

rc = pthread_attr_getdetachstate(&thread_attr, &detachstate);
if (rc != 0) {
    /* 处理错误 */
    ...
} else {
    /* detachstate 的合法值为：
     * PTHREAD_CREATE_DETACHED 或 PTHREAD_CREATE_JOINABLE
     */
    ...
}
```

## 应用程序使用

无。

## 基本原理

如果实现检测到传递给 `pthread_attr_getdetachstate()` 或 `pthread_attr_setdetachstate()` 的 `attr` 参数值不引用已初始化的线程属性对象，建议函数应失败并报告 [EINVAL] 错误。

## 未来方向

无。

## 另请参阅

- `pthread_attr_destroy()`
- `pthread_attr_getstacksize()`
- `pthread_create()`
- `<pthread.h>`

## 变更历史

### 首次发布于 Issue 5
为与 POSIX 线程扩展对齐而包含。

### Issue 6
- `pthread_attr_setdetachstate()` 和 `pthread_attr_getdetachstate()` 函数被标记为线程选项的一部分。
- 规范性文本已更新，避免在应用程序要求中使用"必须"一词。
- 应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/72，在示例部分添加示例。
- 应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/73，更新错误部分以包含可选的 [EINVAL] 错误。

### Issue 7
- `pthread_attr_setdetachstate()` 和 `pthread_attr_getdetachstate()` 函数从线程选项移动到基础。
- 移除了未初始化线程属性对象的 [EINVAL] 错误；此条件导致未定义行为。

---
