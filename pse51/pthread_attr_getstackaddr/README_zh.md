# pthread_attr_getstackaddr, pthread_attr_setstackaddr

## 概要 (SYNOPSIS)

```c
[OB] #include <pthread.h>

int pthread_attr_getstackaddr(const pthread_attr_t *restrict attr,
                             void **restrict stackaddr);

int pthread_attr_setstackaddr(pthread_attr_t *attr, void *stackaddr);
```

## 描述 (DESCRIPTION)

`pthread_attr_getstackaddr()` 和 `pthread_attr_setstackaddr()` 函数分别用于获取和设置 `attr` 对象中的线程创建 `stackaddr` 属性。

`stackaddr` 属性指定用于所创建线程栈的存储位置。该存储的大小应至少为 {PTHREAD_STACK_MIN}。

## 返回值 (RETURN VALUE)

成功完成后，`pthread_attr_getstackaddr()` 和 `pthread_attr_setstackaddr()` 应返回值 0；否则，应返回错误码以指示错误。

如果成功，`pthread_attr_getstackaddr()` 函数将 `stackaddr` 属性值存储在 `stackaddr` 中。

## 错误 (ERRORS)

这些函数可能会在以下情况下失败：

- **[EINVAL]**
  `attr` 指定的值不引用已初始化的线程属性对象。

这些函数不应返回 [EINTR] 错误码。

## 应用程序使用 (APPLICATION USAGE)

`stackaddr` 属性的规范存在一些歧义，使得这些接口的可移植使用变得不可能。将单个地址参数描述为"栈"并未指定地址与该地址所暗示的"栈"之间的特定关系。例如，该地址可以被视为用作栈的缓冲区的低内存地址，或者可以被视为用作新线程的初始栈指针寄存器值的地址。这两者并不相同，除非是在栈从低内存向高内存"向上"增长的机器上，并且在该机器上"推入"操作首先将值存储在内存中，然后递增栈指针寄存器。此外，在栈从高内存向低内存"向下"增长的机器上，将地址解释为"低内存"地址需要确定栈的预期大小。IEEE Std 1003.1-2001 引入了新的接口 [`pthread_attr_setstack()`](pthread_attr_setstack.html) 和 [`pthread_attr_getstack()`](pthread_attr_getstack.html) 来解决这些歧义。

在成功调用 `pthread_attr_setstackaddr()` 后，`stackaddr` 参数指定的存储区域由实现控制，如*应用程序管理的线程栈的使用*中所述。

## 另请参见 (SEE ALSO)

- [`pthread_attr_destroy()`](pthread_attr_destroy.html)
- [`pthread_attr_getdetachstate()`](pthread_attr_getdetachstate.html)
- [`pthread_attr_getstack()`](pthread_attr_getstack.html)
- [`pthread_attr_getstacksize()`](pthread_attr_getstacksize.html)
- [`pthread_attr_setstack()`](pthread_attr_setstack.html)
- [`pthread_create()`](pthread_create.html)
- `<limits.h>`
- `<pthread.h>`

## 更改历史 (CHANGE HISTORY)

首次在 Issue 5 中发布。包含在内以与 POSIX 线程扩展对齐。

### Issue 6

`pthread_attr_getstackaddr()` 和 `pthread_attr_setstackaddr()` 函数被标记为线程和线程栈地址属性选项的一部分。

为了与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getstackaddr()` 原型添加了 `restrict` 关键字。

这些函数被标记为过时。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/85，更新了应用程序使用部分以引用*应用程序管理的线程栈的使用*。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/86，更新了错误部分以包含当 `attr` 引用未初始化的线程属性对象时的可选错误。

---
