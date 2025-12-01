# pthread_setcancelstate, pthread_setcanceltype, pthread_testcancel — 设置可取消状态

## 概要

```c
#include <pthread.h>

int pthread_setcancelstate(int state, int *oldstate);
int pthread_setcanceltype(int type, int *oldtype);
void pthread_testcancel(void);
```

## 描述

`pthread_setcancelstate()` 函数应原子性地将调用线程的可取消状态设置为指定的 `state`，并在 `oldstate` 引用的位置返回之前的可取消状态。`state` 的合法值为 `PTHREAD_CANCEL_ENABLE` 和 `PTHREAD_CANCEL_DISABLE`。

`pthread_setcanceltype()` 函数应原子性地将调用线程的可取消类型设置为指定的 `type`，并在 `oldtype` 引用的位置返回之前的可取消类型。`type` 的合法值为 `PTHREAD_CANCEL_DEFERRED` 和 `PTHREAD_CANCEL_ASYNCHRONOUS`。

任何新创建线程（包括首次调用 `main()` 的线程）的可取消状态和类型应分别为 `PTHREAD_CANCEL_ENABLE` 和 `PTHREAD_CANCEL_DEFERRED`。

`pthread_testcancel()` 函数应在调用��程中创建一个取消点。如果取消被禁用，`pthread_testcancel()` 函数应没有效果。

`pthread_setcancelstate()` 函数应为异步信号安全。

## 返回值

如果成功，`pthread_setcancelstate()` 和 `pthread_setcanceltype()` 函数应返回零；否则，应返回错误编号以指示错误。

## 错误

`pthread_setcancelstate()` 函数可能在以下情况下失败：

- **[EINVAL]**
  - 指定的状态不是 `PTHREAD_CANCEL_ENABLE` 或 `PTHREAD_CANCEL_DISABLE`。

`pthread_setcanceltype()` 函数可能在以下情况下失败：

- **[EINVAL]**
  - 指定的类型不是 `PTHREAD_CANCEL_DEFERRED` 或 `PTHREAD_CANCEL_ASYNCHRONOUS`。

这些函数不应返回 [EINTR] 错误码。

## 示例

无。

## 应用程序用法

要为可以在可取消线程中安全运行的异步信号编写信号处理程序，必须使用 `pthread_setcancelstate()` 在信号处理程序执行的任何取消点调用期间禁用取消。但是，标准的早期版本不允许严格符合的应用程序从信号处理程序调用 `pthread_setcancelstate()`，因为它不要求是异步信号安全的。在 `pthread_setcancelstate()` 不是异步信号安全的不符合规范的实现上，替代方案是确保在执行不是异步取消安全的函数期间阻塞相应的信号，或者在可能传递这些信号时禁用取消。

## 基本原理

`pthread_setcancelstate()` 和 `pthread_setcanceltype()` 函数控制线程可能被异步取消的点。为了使取消控制能够以模块化方式使用，需要遵循一些规则。

对象可以看作是过程的泛化。它是作为单元编写的一组过程和全局变量，由对象不知道的客户端调用。对象可能依赖于其他对象。

首先，取消只应在进入对象时禁用，绝不应显式启用。退出对象时，应始终将取消状态恢复到进入对象时的值。

这是从模块化论证得出的：如果一个对象的客户端（或使用该对象的对象的客户端）禁用了取消，这是因为客户端不关心如果线程在执行某些操作序列时被取消的清理工作。如果在这种状态下调用对象并启用取消，并且该线程有待处理的取消请求，那么线程将被取消，这与禁用的客户端的愿望相违背。

其次，取消类型可以在进入对象时显式设置为延迟或异步。但与取消状态一样，退出对象时应始终将取消类型恢复到进入对象时的值。

最后，只有取消安全的函数才能从可异步取消的线程中调用。

## 未来方向

无。

## 另请参阅

- [`pthread_cancel()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_cancel.html)
- XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 更改历史

### 在第5期中首次发布
为了与 POSIX 线程扩展对齐而包含。

### 第6期
`pthread_setcancelstate()`、`pthread_setcanceltype()` 和 `pthread_testcancel()` 函数被标记为线程选项的一部分。

### 第7期
`pthread_setcancelstate()`、`pthread_setcanceltype()` 和 `pthread_testcancel()` 函数从线程选项移动到基本规范。

应用了 POSIX.1-2008 技术勘误 2，XSH/TC2-2008/0294 [622] 和 XSH/TC2-2008/0295 [615]。

### 第8期
应用了 Austin Group 缺陷 841，要求 `pthread_setcancelstate()` 为异步信号安全。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*