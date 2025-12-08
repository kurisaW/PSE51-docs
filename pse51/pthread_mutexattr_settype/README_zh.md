# pthread_mutexattr_gettype, pthread_mutexattr_settype - 获取和设置互斥锁类型属性

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutexattr_gettype(const pthread_mutexattr_t *restrict attr,
                             int *restrict type);

int pthread_mutexattr_settype(pthread_mutexattr_t *attr, int type);
```

## DESCRIPTION

`pthread_mutexattr_gettype()` 和 `pthread_mutexattr_settype()` 函数分别用于获取和设置互斥锁类型属性。该属性在这些函数的 type 参数中设置。类型属性的默认值为 PTHREAD_MUTEX_DEFAULT。

互斥锁的类型包含在互斥锁属性的 type 属性中。有效的互斥锁类型包括：

- PTHREAD_MUTEX_NORMAL
- PTHREAD_MUTEX_ERRORCHECK
- PTHREAD_MUTEX_RECURSIVE
- PTHREAD_MUTEX_DEFAULT

互斥锁类型会影响锁定和解锁互斥锁的调用行为。详细信息请参见 `pthread_mutex_lock()`。实现可以将 PTHREAD_MUTEX_DEFAULT 映射到其他互斥锁类型之一。

如果传递给 `pthread_mutexattr_gettype()` 或 `pthread_mutexattr_settype()` 的 attr 参数值没有引用已初始化的互斥锁属性对象，则行为未定义。

## RETURN VALUE

成功完成后，`pthread_mutexattr_gettype()` 函数应返回零，并将 attr 的类型属性值存储到 type 参数引用的对象中。否则，应返回错误以指示错误。

如果成功，`pthread_mutexattr_settype()` 函数应返回零；否则，应返回错误编号以指示错误。

## ERRORS

`pthread_mutexattr_settype()` 函数在以下情况应失败：

- **[EINVAL]**
  type 值无效。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES

无。

## APPLICATION USAGE

建议应用程序不要将 PTHREAD_MUTEX_RECURSIVE 互斥锁与条件变量一起使用，因为在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中执行的隐式解锁可能不会实际释放互斥锁（如果它已被多次锁定）。如果发生这种情况，没有其他线程能够满足谓词的条件。

## RATIONALE

如果实现检测到传递给 `pthread_mutexattr_gettype()` 或 `pthread_mutexattr_settype()` 的 attr 参数值没有引用已初始化的互斥锁属性对象，建议函数应该失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `pthread_cond_clockwait()`
- `pthread_mutex_lock()`
- `<pthread.h>`

## CHANGE HISTORY

### 首次在 Issue 5 中发布。

### Issue 6

应用 Open Group Corrigendum U033/3。`pthread_mutexattr_gettype()` 的 SYNOPSIS 更新为第一个参数是 `const pthread_mutexattr_t *` 类型。

为与 ISO/IEC 9899:1999 标准保持一致，向 `pthread_mutexattr_gettype()` 原型添加了 `restrict` 关键字。

### Issue 7

`pthread_mutexattr_gettype()` 和 `pthread_mutexattr_settype()` 函数从 XSI 选项移动到 Base。

删除了未初始化互斥锁属性对象的 [EINVAL] 错误；此条件导致未定义行为。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0464 [121]。

### Issue 8

应用 Austin Group Defect 1216，添加 `pthread_cond_clockwait()`。

---
