# pthread_attr_getstacksize, pthread_attr_setstacksize — 获取和设置栈大小属性

## SYNOPSIS

```c
#include <pthread.h>

int pthread_attr_getstacksize(const pthread_attr_t *restrict attr,
                              size_t *restrict stacksize);

int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);
```

## DESCRIPTION

`pthread_attr_getstacksize()` 和 `pthread_attr_setstacksize()` 函数分别用于获取和设置 *attr* 对象中的线程创建 *stacksize*（栈大小）属性。

*stacksize* 属性定义了为所创建线程栈分配的最小栈大小（以字节为单位）。

如果传递给 `pthread_attr_getstacksize()` 或 `pthread_attr_setstacksize()` 的 *attr* 参数值不指向已初始化的线程属性对象，则行为是未定义的。

## RETURN VALUE

成功完成后，`pthread_attr_getstacksize()` 和 `pthread_attr_setstacksize()` 应返回值 0；否则，应返回错误码以指示错误。

如果成功，`pthread_attr_getstacksize()` 函数会将 *stacksize* 属性值存储在 *stacksize* 中。

## ERRORS

`pthread_attr_setstacksize()` 函数在以下情况下应失败：

- **[EINVAL]**
  *stacksize* 的值小于 {PTHREAD_STACK_MIN} 或超过系统强制的限制。

这些函数不应返回 [EINTR] 错误码。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

如果实现检测到传递给 `pthread_attr_getstacksize()` 或 `pthread_attr_setstacksize()` 的 *attr* 参数值不指向已初始化的线程属性对象，建议函数应失败并报告 [EINVAL] 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_attr_getdetachstate()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_getdetachstate.html)
- [`pthread_create()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_create.html)
- XBD [`<limits.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/limits.h.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 5
为了与 POSIX 线程扩展对齐而包含。

### Issue 6
- `pthread_attr_getstacksize()` 和 `pthread_attr_setstacksize()` 函数被标记为线程和线程栈大小属性选项的一部分。
- 为了与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getstacksize()` 原型添加了 **restrict** 关键字。
- 应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/43，将 SYNOPSIS 中的边距代码从 TSA 更正为 TSS，并将 CHANGE HISTORY 从"线程栈地址属性"选项更新为"线程栈大小属性"选项。
- 应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/87，更新 ERRORS 部分以包含当 *attr* 指向未初始化线程属性对象时的可选错误。

### Issue 7
- `pthread_attr_getstacksize()` 和 `pthread_attr_setstacksize()` 函数仅被标记为线程栈大小属性选项的一部分，因为线程选项现在是基础的一部分。
- 移除了未初始化线程属性对象的 [EINVAL] 错误；此条件导致未定义行为。
- 应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0265 [757]。

---
