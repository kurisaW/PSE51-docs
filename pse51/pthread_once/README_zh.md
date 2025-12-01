# pthread_once — 动态包初始化

## 概要

```c
#include <pthread.h>

int pthread_once(pthread_once_t *once_control,
                 void (*init_routine)(void));

pthread_once_t once_control = PTHREAD_ONCE_INIT;
```

## 描述

进程中任何线程对给定 `once_control` 的第一次 `pthread_once()` 调用应该调用不带参数的 `init_routine`。随后使用相同 `once_control` 的 `pthread_once()` 调用不会调用 `init_routine`。从 `pthread_once()` 返回时，`init_routine` 应该已经完成。`once_control` 参数应该确定相关的初始化例程是否已被调用。

`pthread_once()` 函数不是一个取消点。但是，如果 `init_routine` 是一个取消点并且被取消，对 `once_control` 的效果就如同 `pthread_once()` 从未被调用一样。

如果对 `init_routine` 的调用被 `longjmp()` 或 `siglongjmp()` 调用终止，行为是未定义的。

常量 PTHREAD_ONCE_INIT 在 `<pthread.h>` 头文件中定义。

如果 `once_control` 具有自动存储期或未被 PTHREAD_ONCE_INIT 初始化，`pthread_once()` 的行为是未定义的。

## 返回值

成功完成后，`pthread_once()` 应返回零；否则，应返回错误编号以指示错误。

## 错误

`pthread_once()` 函数不应返回 [EINTR] 错误代码。

## 应用用法

如果 `init_routine` 递归地使用相同的 `once_control` 调用 `pthread_once()`，递归调用不会调用指定的 `init_routine`，因此指定的 `init_routine` 不会完成，因此对 `pthread_once()` 的递归调用不会返回。在 `init_routine` 中使用 `longjmp()` 或 `siglongjmp()` 跳转到 `init_routine` 外部的点会阻止 `init_routine` 返回。

## 原理

一些 C 库是为动态初始化而设计的。也就是说，库的全局初始化在调用库中的第一个过程时执行。在单线程程序中，这通常使用一个静态变量来实现，在进入例程时检查其值，如下所示：

```c
static int random_is_initialized = 0;
extern void initialize_random(void);

int random_function()
{
    if (random_is_initialized == 0) {
        initialize_random();
        random_is_initialized = 1;
    }
    ... /* 初始化后执行的操作。 */
}
```

为了在多线程程序中保持相同的结构，需要一个新的原语。否则，库初始化必须通过在对库的任何使用之前显式调用库导出的初始化函数来完成。

对于多线程进程中的动态库初始化，如果使用初始化标志，则需要保护该标志以防止多个线程同时调用库时对其进行修改。这可以通过使用互斥锁（通过赋值 PTHREAD_MUTEX_INITIALIZER 初始化）来完成。但是，更好的解决方案是使用 `pthread_once()`，它正是为此目的而设计的，如下所示：

```c
#include <pthread.h>
static pthread_once_t random_is_initialized = PTHREAD_ONCE_INIT;
extern void initialize_random(void);

int random_function()
{
    (void) pthread_once(&random_is_initialized, initialize_random);
    ... /* 初始化后执行的操作。 */
}
```

如果实现检测到传递给 `pthread_once()` 的 `once_control` 参数值不引用由 PTHREAD_ONCE_INIT 初始化的 **pthread_once_t** 对象，建议函数应该失败并报告 [EINVAL] 错误。

## 未来方向

无。

## 参见

- `<pthread.h>`

## 变更历史

**首次发布于 Issue 5。** 包含用于与 POSIX 线程扩展对齐。

**Issue 6**
- `pthread_once()` 函数被标记为线程选项的一部分。
- 添加了 [EINVAL] 错误作为"可能失败"情况，如果任一参数无效。

**Issue 7**
- `pthread_once()` 函数从线程选项移动到基础。
- 移除了未初始化 **pthread_once_t** 对象的 [EINVAL] 错误；此条件导致未定义行为。
- 应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0284 [863], XSH/TC2-2008/0285 [874], XSH/TC2-2008/0286 [874], 和 XSH/TC2-2008/0287 [747]。

**Issue 8**
- 应用了 Austin Group Defect 1330，移除了过时的接口。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 IEEE 和 The Open Group*