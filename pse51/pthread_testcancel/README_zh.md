# pthread_setcancelstate, pthread_setcanceltype, pthread_testcancel

## 概要 (SYNOPSIS)

```c
#include <pthread.h>

int pthread_setcancelstate(int state, int *oldstate);
int pthread_setcanceltype(int type, int *oldtype);
void pthread_testcancel(void);
```

## 描述 (DESCRIPTION)

`pthread_setcancelstate()` 函数应原子地设置调用线程的可取消状态为指定的 `state`，并在 `oldstate` 引用的位置返回先前的可取消状态。`state` 的合法值为 `PTHREAD_CANCEL_ENABLE` 和 `PTHREAD_CANCEL_DISABLE`。

`pthread_setcanceltype()` 函数应原子地设置调用线程的可取消类型为指定的 `type`，并在 `oldtype` 引用的位置返回先前的可取消类型。`type` 的合法值为 `PTHREAD_CANCEL_DEFERRED` 和 `PTHREAD_CANCEL_ASYNCHRONOUS`。

任何新创建线程（包括首次调用 `main()` 的线程）的可取消状态和类型应分别为 `PTHREAD_CANCEL_ENABLE` 和 `PTHREAD_CANCEL_DEFERRED`。

`pthread_testcancel()` 函数应在调用线程中创建一个取消点。如果取消被禁用，`pthread_testcancel()` 函数应无效。

`pthread_setcancelstate()` 函数应为异步信号安全函数。

## 返回值 (RETURN VALUE)

如果成功，`pthread_setcancelstate()` 和 `pthread_setcanceltype()` 函数应返回零；否则，应返回错误码以指示错误。

## 错误 (ERRORS)

`pthread_setcancelstate()` 函数可能失败，如果：

- **[EINVAL]**
  - 指定的状态不是 `PTHREAD_CANCEL_ENABLE` 或 `PTHREAD_CANCEL_DISABLE`。

`pthread_setcanceltype()` 函数可能失败，如果：

- **[EINVAL]**
  - 指定的类型不是 `PTHREAD_CANCEL_DEFERRED` 或 `PTHREAD_CANCEL_ASYNCHRONOUS`。

这些函数不应返回 `[EINTR]` 错误码。

---

*以下章节为参考信息。*

## 示例 (EXAMPLES)

无。

## 应用程序使用 (APPLICATION USAGE)

为了为可以在可取消线程中安全运行的异步信号编写信号处理程序，必须使用 `pthread_setcancelstate()` 在信号处理程序执行的任何可能作为取消点的调用期间禁用取消。但是，标准的早期版本不允许严格符合的应用程序从信号处理程序调用 `pthread_setcancelstate()`，因为它不要求是异步信号安全的。在 `pthread_setcancelstate()` 不是异步信号安全的不符合实现上，替代方案是确保在执行非异步取消安全函数期间阻塞相应的信号，或者在可能传递这些信号时禁用取消。

## 原理 (RATIONALE)

`pthread_setcancelstate()` 和 `pthread_setcanceltype()` 函数控制线程可能被异步取消的时间点。为了使取消控制能够以模块化方式使用，需要遵循一些规则。

对象可以被认为是过程的泛化。它是作为单元编写的一组过程和全局变量，并被对象不知道的客户端调用。对象可能依赖于其他对象。

首先，可取消性只能在进入对象时禁用，绝不能显式启用。在退出对象时，可取消状态应始终恢复到进入对象时的值。

这遵循模块化参数：如果对象的客户端（或使用该对象的对象的客户端）禁用了可取消性，这是因为客户端不关心如果线程在执行某些操作序列时被取消时进行清理。如果在这种状态下调用对象并启用了可取消性，并且该线程有待处理的取消请求，那么线程被取消，这与禁用客户端的愿望相反。

其次，可取消类型可以在进入对象时显式设置为延迟或异步。但与可取消状态一样，在退出对象时，可取消类型应始终恢复到进入对象时的值。

最后，只有取消安全的函数才能从可异步取消的线程中调用。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 另请参见 (SEE ALSO)

- [`pthread_cancel()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cancel.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 更改历史 (CHANGE HISTORY)

首次在 Issue 5 中发布。包含用于与 POSIX 线程扩展对齐。

### Issue 6

`pthread_setcancelstate()`、`pthread_setcanceltype()` 和 `pthread_testcancel()` 函数被标记为线程选项的一部分。

### Issue 7

`pthread_setcancelstate()`、`pthread_setcanceltype()` 和 `pthread_testcancel()` 函数从线程选项移动到基础。

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0294 [622] 和 XSH/TC2-2008/0295 [615] 被应用。

### Issue 8

Austin Group Defect 841 被应用，要求 `pthread_setcancelstate()` 为异步信号安全。

*参考文本结束。*

---
