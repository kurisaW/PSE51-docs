# pthread_setcancelstate, pthread_setcanceltype, pthread_testcancel

## SYNOPSIS

```c
#include <pthread.h>

int pthread_setcancelstate(int state, int *oldstate);
int pthread_setcanceltype(int type, int *oldtype);
void pthread_testcancel(void);
```

## DESCRIPTION

`pthread_setcancelstate()` 函数应该原子性地将调用线程的可取消性状态设置为指定的 `state`，并在 `oldstate` 引用的位置返回之前的可取消性状态。`state` 的合法值为 `PTHREAD_CANCEL_ENABLE` 和 `PTHREAD_CANCEL_DISABLE`。

`pthread_setcanceltype()` 函数应该原子性地将调用线程的可取消性类型设置为指定的 `type`，并在 `oldtype` 引用的位置返回之前的可取消性类型。`type` 的合法值为 `PTHREAD_CANCEL_DEFERRED` 和 `PTHREAD_CANCEL_ASYNCHRONOUS`。

任何新创建的线程（包括首次调用 `main()` 的线程）的可取消性状态和类型应该分别为 `PTHREAD_CANCEL_ENABLE` 和 `PTHREAD_CANCEL_DEFERRED`。

`pthread_testcancel()` 函数应该在调用线程中创建一个取消点。如果可取消性被禁用，`pthread_testcancel()` 函数应该没有效果。

`pthread_setcancelstate()` 函数应该是异步信号安全的。

## RETURN VALUE

如果成功，`pthread_setcancelstate()` 和 `pthread_setcanceltype()` 函数应该返回零；否则，应该返回错误编号以指示错误。

## ERRORS

`pthread_setcancelstate()` 函数可能在以下情况失败：

- **[EINVAL]**
  - 指定的状态不是 `PTHREAD_CANCEL_ENABLE` 或 `PTHREAD_CANCEL_DISABLE`。

`pthread_setcanceltype()` 函数可能在以下情况失败：

- **[EINVAL]**
  - 指定的类型不是 `PTHREAD_CANCEL_DEFERRED` 或 `PTHREAD_CANCEL_ASYNCHRONOUS`。

这些函数不应该返回错误代码 `[EINTR]`。

## EXAMPLES

无。

## APPLICATION USAGE

为了编写可以在可取消线程中安全运行的异步信号信号处理器，必须使用 `pthread_setcancelstate()` 在信号处理器执行的任何可能是取消点的调用期间禁用取消。然而，标准的早期版本不允许严格符合的应用程序从信号处理器调用 `pthread_setcancelstate()`，因为它不被要求是异步信号安全的。在 `pthread_setcancelstate()` 不是异步信号安全的非符合实现中，替代方案是要么确保在执行不是异步取消安全的函数期间阻塞相应的信号，要么在可能传递这些信号时禁用取消。

## RATIONALE

`pthread_setcancelstate()` 和 `pthread_setcanceltype()` 函数控制线程可能被异步取消的时机。为了使取消控制能够以模块化方式使用，需要遵循一些规则。

对象可以被视为过程的泛化。它是作为一个单元编写的一组过程和全局变量，被对象不知道的客户端调用。对象可能依赖于其他对象。

首先，可取消性应该只在进入对象时禁用，永远不应该显式启用。在退出对象时，可取消性状态应该始终恢复到进入对象时的值。

这遵循模块化参数：如果对象的客户端（或使用该对象的对象的客户端）禁用了可取消性，这是因为客户端不希望在线程在执行某些操作序列时被取消而担心清理工作。如果在这样的状态下调用对象并且它启用了可取消性，并且该线程有待处理的取消请求，那么线程被取消，这与禁用它的客户端的愿望相反。

其次，在进入对象时，可取消性类型可以显式设置为 *延迟* 或 *异步*。但与可取消性状态一样，在退出对象时，可取消性类型应该始终恢复到进入对象时的值。

最后，只能从异步可取消的线程中调用取消安全的函数。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_cancel()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cancel.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

### 首次发布于 Issue 5
为了与 POSIX 线程扩展保持一致而包含。

### Issue 6
`pthread_setcancelstate()`、`pthread_setcanceltype()` 和 `pthread_testcancel()` 函数被标记为线程选项的一部分。

### Issue 7
`pthread_setcancelstate()`、`pthread_setcanceltype()` 和 `pthread_testcancel()` 函数从线程选项移动到基础。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0294 [622] 和 XSH/TC2-2008/0295 [615]。

### Issue 8
应用了 Austin Group Defect 841，要求 `pthread_setcancelstate()` 是异步信号安全的。

---
