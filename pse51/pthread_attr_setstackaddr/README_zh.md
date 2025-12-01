# pthread_attr_getstackaddr, pthread_attr_setstackaddr

## NAME

pthread_attr_getstackaddr, pthread_attr_setstackaddr - 获取和设置栈地址属性

## SYNOPSIS

```c
[OB] int pthread_attr_getstackaddr(const pthread_attr_t *restrict attr,
                                  void **restrict stackaddr);
int pthread_attr_setstackaddr(pthread_attr_t *attr, void *stackaddr);
```

## DESCRIPTION

`pthread_attr_getstackaddr()` 和 `pthread_attr_setstackaddr()` 函数分别用于获取和设置 attr 对象中的线程创建栈地址属性。

栈地址属性指定用于所创建线程栈的存储位置。该存储的大小应至少为 {PTHREAD_STACK_MIN}��

## RETURN VALUE

成功完成后，`pthread_attr_getstackaddr()` 和 `pthread_attr_setstackaddr()` 应返回值 0；否则，应返回错误码以指示错误。

如果成功，`pthread_attr_getstackaddr()` 函数会将栈地址属性值存储在 stackaddr 中。

## ERRORS

这些函数可能在以下情况下失败：

- **[EINVAL]**
  - attr 指定的值不引用已初始化的线程属性对象。

这些函数不应返回错误码 [EINTR]。

## EXAMPLES

无。

## APPLICATION USAGE

栈地址属性的规范存在若干歧义，使得这些接口的可移植使用成为不可能。将单个地址参数描述为"栈"并未指定地址与该地址所暗示的"栈"之间的特定关系。例如，该地址可以被视为用作栈的缓冲区的低内存地址，也可以被视为用作新线程的初始栈指针寄存器值的地址。这两种情况只有在栈从低内存向高内存"向上"增长，并且"push"操作先在内存中存储值然后递增栈指针寄存器的机器上才是相同的。此外，在栈从高内存向低内存"向下"增长的机器上，将地址解释为"低内存"地址需要确定栈的预期大小。IEEE Std 1003.1-2001 引入了新接口 [`pthread_attr_setstack()`](pthread_attr_setstack.md) 和 [`pthread_attr_getstack()`](pthread_attr_getstack.md) 来解决这些歧义。

在成功调用 `pthread_attr_setstackaddr()` 后，由 stackaddr 参数指定的存储区域由实现控制，如"应用程序管理线程栈的使用"中所述。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_attr_destroy()`](pthread_attr_destroy.md)
- [`pthread_attr_getdetachstate()`](pthread_attr_getdetachstate.md)
- [`pthread_attr_getstack()`](pthread_attr_getstack.md)
- [`pthread_attr_getstacksize()`](pthread_attr_getstacksize.md)
- [`pthread_attr_setstack()`](pthread_attr_setstack.md)
- [`pthread_create()`](pthread_create.md)
- IEEE Std 1003.1-2001 基础定义卷，`<limits.h>`，`<pthread.h>`

## CHANGE HISTORY

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含。

### Issue 6

`pthread_attr_getstackaddr()` 和 `pthread_attr_setstackaddr()` 函数被标记为线程和线程栈地址属性选项的一部分。

为与 ISO/IEC 9899:1999 标准对齐，向 `pthread_attr_getstackaddr()` 原型添加了 **restrict** 关键字。

这些函数被标记为过时。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/85，更新 APPLICATION USAGE 部分以引用"应用程序管理线程栈的使用"。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/86，更新 ERRORS 部分以包含当 attr 引用未初始化线程属性对象时的可选错误。

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX® 是 IEEE 的注册商标。*