# pthread_mutex_lock

## 概要

```c
#include <pthread.h>

int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

## 描述

通过调用 `pthread_mutex_lock()` 来锁定 *mutex* 引用的互斥锁对象，该调用返回零或 [EOWNERDEAD]。如果互斥锁已被其他线程锁定，调用线程将被阻塞，直到互斥锁变为可用。此操作返回时，*mutex* 引用的互斥锁对象将处于锁定状态，调用线程为其所有者。如果线程尝试重新锁定它已经锁定的互斥锁，`pthread_mutex_lock()` 的行为将如下表 **重新锁定** 列所述。如果线程尝试解锁它未锁定的互斥锁或已解锁的互斥锁，`pthread_mutex_unlock()` 的行为将如下表 **非所有者解锁** 列所述。

| 互斥锁类型 | 健壮性 | 重新锁定 | 非所有者解锁 |
|------------|------------|--------|----------------------|
| NORMAL | 非健壮 | 死锁 | 未定义行为 |
| NORMAL | 健壮 | 死锁 | 返回错误 |
| ERRORCHECK | 任意 | 返回错误 | 返回错误 |
| RECURSIVE | 任意 | 递归（见下文） | 返回错误 |
| DEFAULT | 非健壮 | 未定义行为† | 未定义行为† |
| DEFAULT | 健壮 | 未定义行为† | 返回错误 |

† 如果互斥锁类型为 PTHREAD_MUTEX_DEFAULT，`pthread_mutex_lock()` 的行为可能对应上表中其他三种标准互斥锁类型之一。如果不对应这三种类型中的任何一种，则标记为 † 的情况下的行为是未定义的。

当表中表示递归行为时，互斥锁应维护锁定计数概念。当线程首次成功获取互斥锁时，锁定计数应设置为一。每次线程重新锁定此互斥锁时，锁定计数应加一。每次线程解锁互斥锁时，锁定计数应减一。当锁定计数达到零时，互斥锁应变为可供其他线程获取的状态。

`pthread_mutex_trylock()` 函数应等效于 `pthread_mutex_lock()`，但如果 *mutex* 引用的互斥锁对象当前被锁定（由任何线程，包括当前线程锁定），调用应立即返回。如果互斥锁类型为 PTHREAD_MUTEX_RECURSIVE 且互斥锁当前由调用线程拥有，互斥锁锁定计数应加一，`pthread_mutex_trylock()` 函数应立即返回成功。

`pthread_mutex_unlock()` 函数应释放 *mutex* 引用的互斥锁对象。互斥锁的释放方式取决于互斥锁的类型属性。如果在调用 `pthread_mutex_unlock()` 时有线程阻塞在 *mutex* 引用的互斥锁对象上，导致互斥锁变为可用，调度策略应决定哪个线程应获取该互斥锁。

（对于 PTHREAD_MUTEX_RECURSIVE 互斥锁，当计数达到零且调用线程不再拥有此互斥锁的任何锁时，互斥锁应变为可用。）

如果信号被传递给等待互斥锁的线程，从信号处理程序返回后，线程应恢复等待互斥锁，就像未被中断一样。

如果 *mutex* 是健壮互斥锁且包含拥有线程的进程在持有互斥锁时终止，调用 `pthread_mutex_lock()` 应返回错误值 [EOWNERDEAD]。如果 *mutex* 是健壮互斥锁且拥有线程在持有互斥锁时终止，即使拥有线程所在的进程尚未终止，调用 `pthread_mutex_lock()` 也可能返回错误值 [EOWNERDEAD]。在这些情况下，互斥锁应被调用线程锁定，但其保护的状态被标记为不一致。应用程序应确保状态变得一致以供重用，完成后调用 `pthread_mutex_consistent()`。如果应用程序无法恢复状态，它应在未先调用 `pthread_mutex_consistent()` 的情况下解锁互斥锁，之后该互斥锁被标记为永久不可用。

如果 *mutex* 不引用已初始化的互斥锁对象，`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 的行为是未定义的。

## 返回值

如果成功，`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 函数应返回零；否则，应返回错误编号以指示错误。

## 错误

`pthread_mutex_lock()` 和 `pthread_mutex_trylock()` 函数在以下情况下应失败：

- **[EAGAIN]**
  - 无法获取互斥锁，因为 *mutex* 的递归锁最大数量已被超过。
  - 互斥锁是健壮互斥锁，且可用的健壮互斥锁拥有的系统资源将被超过。

- **[EINVAL]** [RPP|TPP]
  - *mutex* 是使用协议属性值 PTHREAD_PRIO_PROTECT 创建的，且调用线程的优先级高于互斥锁的当前优先级上限。

- **[ENOTRECOVERABLE]**
  - 互斥锁保护的状态不可恢复。

- **[EOWNERDEAD]**
  - 互斥锁是健壮互斥锁，且包含先前拥有线程的进程在持有互斥锁时终止。互斥锁应由调用线程获取，并由新所有者负责使状态一致。

`pthread_mutex_lock()` 函数在以下情况下应失败：

- **[EDEADLK]**
  - 互斥锁类型为 PTHREAD_MUTEX_ERRORCHECK，且当前线程已拥有该互斥锁。

`pthread_mutex_trylock()` 函数在以下情况下应失败：

- **[EBUSY]**
  - 无法获取 *mutex*，因为它已被锁定。

`pthread_mutex_unlock()` 函数在以下情况下应失败：

- **[EPERM]**
  - 互斥锁类型为 PTHREAD_MUTEX_ERRORCHECK 或 PTHREAD_MUTEX_RECURSIVE，或者互斥锁是健壮互斥锁，且当前线程不拥有该互斥锁。

`pthread_mutex_lock()` 和 `pthread_mutex_trylock()` 函数在以下情况下可能失败：

- **[EOWNERDEAD]**
  - 互斥锁是健壮互斥锁，且先前拥有线程在持有互斥锁时终止。互斥锁应由调用线程获取，并由新所有者负责使状态一致。

`pthread_mutex_lock()` 函数在以下情况下可能失败：

- **[EDEADLK]**
  - 检测到死锁条件。

这些函数不应返回错误代码 [EINTR]。

---

*以下章节为信息性内容。*

## 示例

无。

## 应用程序用法

假设非零返回值为错误的应用程序需要更新才能与健壮互斥锁一起使用，因为线程获取保护当前不一致状态的互斥锁时的有效返回值是 [EOWNERDEAD]。由于排除了出现此类错误的可能性而不检查错误返回值的应用程序不应使用健壮互斥锁。如果应用程序应与普通和健壮互斥锁一起工作，它应检查所有返回值的错误条件，并在必要时采取适当操作。

## 原理

互斥锁对象旨在作为底层原语，可基于此构建其他线程同步函数。因此，互斥锁的实现应尽可能高效，这对接口上可用的功能有影响。

互斥锁函数和互斥锁属性的特定默认设置是出于不排除互斥锁锁定和解锁的快速内联实现的愿望而驱动的。

由于大多数属性仅在线程即将被阻塞时才需要检查，因此使用属性不会减慢（常见的）互斥锁锁定情况。

同样，虽然能够提取互斥锁所有者的线程 ID 可能是可取的，但这需要在锁定每个互斥锁时存储当前线程 ID，这可能导致不可接受的性能开销。类似参数适用于 `mutex_tryunlock` 操作。

有关扩展互斥锁类型的进一步原理，请参见 XRAT *线程扩展*。

如果实现检测到 *mutex* 参数指定的值不引用已初始化的互斥锁对象，建议函数应失败并报告 [EINVAL] 错误。

## 未来方向

无。

## 另请参阅

[`pthread_mutex_clocklock()`](pthread_mutex_clocklock.html), [`pthread_mutex_consistent()`](pthread_mutex_consistent.html), [`pthread_mutex_destroy()`](pthread_mutex_destroy.html), [`pthread_mutexattr_getrobust()`](pthread_mutexattr_getrobust.html)

XBD *4.15.2 内存同步*, `<pthread.h>`

## 变更历史

首次在 Issue 5 中发布。为与 POSIX 线程扩展对齐而包含。

### Issue 6

`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 函数被标记为线程选项的一部分。

以下对 POSIX 实现的新要求源自与单一 UNIX 规范的对齐：

- 定义了尝试重新锁定互斥锁时的行为。

[`pthread_mutex_timedlock()`](pthread_mutex_timedlock.html) 函数被添加到另请参阅部分，以便与 IEEE Std 1003.1d-1999 对齐。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/98，更新错误部分，使 [EDEADLK] 错误包括死锁条件检测。原理部分也被重新措辞，以考虑非 XSI 一致系统。

### Issue 7

应用 SD5-XSH-ERN-43，将 [EINVAL] 错误的"应失败"情况标记为依赖于线程优先级保护选项。

根据 The Open Group Technical Standard, 2006, Extended API Set Part 3 进行更改。

`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 函数从线程选项移至基础。

以下扩展互斥锁类型从 XSI 选项移至基础：

```
PTHREAD_MUTEX_NORMAL
PTHREAD_MUTEX_ERRORCHECK
PTHREAD_MUTEX_RECURSIVE
PTHREAD_MUTEX_DEFAULT
```

更新描述以阐明 *mutex* 不引用已初始化互斥锁时的行为。

更新错误部分以正确处理所有各种互斥锁类型。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0461 [121], XSH/TC1-2008/0462 [92,428], 和 XSH/TC1-2008/0463 [121]。

### Issue 8

应用 Austin Group Defect 354，为超过可用的健壮互斥锁拥有的系统资源添加 [EAGAIN] 错误。

应用 Austin Group Defect 1115，将"the thread"更改为"the calling thread"。

---
