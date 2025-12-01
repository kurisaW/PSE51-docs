# pthread_mutex_lock, pthread_mutex_trylock, pthread_mutex_unlock

## NAME

pthread_mutex_lock, pthread_mutex_trylock, pthread_mutex_unlock — 锁定和解锁互斥锁

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

## DESCRIPTION

通过调用 `pthread_mutex_lock()` 并返回零或 `[EOWNERDEAD]`，`mutex` 引用的互斥锁对象应当被锁定。如果互斥锁已被另一个线程锁定，调用线程应当阻塞，直到互斥锁变为可用。此操作应当在 `mutex` 引用的互斥锁对象处于锁定状态且调用线程为其所有者时返回。如果一个线程尝试重新锁定一个已经锁定的互斥锁，`pthread_mutex_lock()` 的行为应如下表**重新锁定**列所述。如果一个线程尝试解锁一个未锁定的或未由其锁定的互斥锁，`pthread_mutex_unlock()` 的行为应如下表**非所有者解锁**列所述。

| 互斥锁类型 | 健壮性 | 重新锁定 | 非所有者解锁 |
|------------|------------|--------|-----------------------|
| NORMAL | 非健壮 | 死锁 | 未定义行为 |
| NORMAL | 健壮 | 死锁 | 返回错误 |
| ERRORCHECK | 任意 | 返回错误 | 返回错误 |
| RECURSIVE | 任意 | 递归（见下文） | 返回错误 |
| DEFAULT | 非健壮 | 未定义行为† | 未定义行为† |
| DEFAULT | 健壮 | 未定义行为† | 返回错误 |

† 如果互斥锁类型为 `PTHREAD_MUTEX_DEFAULT`，`pthread_mutex_lock()` 的行为可能对���上表中所述的三种其他标准互斥锁类型之一。如果它不对应这三种类型之一，则标记为 † 的行为是未定义的。

当表指示递归行为时，互斥锁应维护锁计数的概念。当线程首次成功获取互斥锁时，锁计数应设置为一。每次线程重新锁定此互斥锁时，锁计数应增加一。每次线程解锁互斥锁时，锁计数应减少一。当锁计数达到零时，互斥锁应变为可供其他线程获取。

`pthread_mutex_trylock()` 函数应等同于 `pthread_mutex_lock()`，但如果 `mutex` 引用的互斥锁对象当前被锁定（由任何线程，包括当前线程），调用应立即返回。如果互斥锁类型为 `PTHREAD_MUTEX_RECURSIVE` 且互斥锁当前由调用线程拥有，互斥锁锁计数应增加一，且 `pthread_mutex_trylock()` 函数应立即返回成功。

`pthread_mutex_unlock()` 函数应释放 `mutex` 引用的互斥锁对象。释放互斥锁的方式取决于互斥锁的类型属性。当调用 `pthread_mutex_unlock()` 时，如果有线程阻塞在 `mutex` 引用的互斥锁对象上，导致互斥锁变为可用，调度策略应决定哪个线程应获取互斥锁。

（在 `PTHREAD_MUTEX_RECURSIVE` 互斥锁的情况下，当计数达到零且调用线程不再对此互斥锁有任何锁定时，互斥锁应变为可用。）

如果信号被传递给等待互斥锁的线程，从信号处理程序返回后，线程应恢复等待互斥锁，就像未被中断一样。

如果 `mutex` 是健壮互斥锁且拥有互斥锁的线程的进程在持有互斥锁时终止，调用 `pthread_mutex_lock()` 应返回错误值 `[EOWNERDEAD]`。如果 `mutex` 是健壮互斥锁且拥有互斥锁的线程在持有互斥锁时终止，即使拥有线程所在的进程尚未终止，调用 `pthread_mutex_lock()` 也可能返回错误值 `[EOWNERDEAD]`。在这些情况下，互斥锁应由调用线程锁定，但它保护的状态被标记为不一致。应用程序应确保状态变得一致以便重用，并在完成时调用 `pthread_mutex_consistent()`。如果应用程序无法恢复状态，它应在未事先调用 `pthread_mutex_consistent()` 的情况下解锁互斥锁，此后互斥锁被标记为永久不可用。

如果 `mutex` 不引用已初始化的互斥锁对象，`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 的行为是未定义的。

## RETURN VALUE

如果成功，`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 函数应返回零；否则，应返回错误号以指示错误。

## ERRORS

`pthread_mutex_lock()` 和 `pthread_mutex_trylock()` 函数应在以下情况下失败：

- **[EAGAIN]** 无法获取互斥锁，因为已超过 `mutex` 的最大递归锁定次数。
- **[EAGAIN]** 互斥锁是健壮互斥锁，且可用于拥有健壮互斥锁的系统资源将被超出。
- **[EINVAL]** `[RPP|TPP]` `mutex` 是用协议属性值为 `PTHREAD_PRIO_PROTECT` 创建的，且调用线程的优先级高于互斥锁的当前优先级上限。
- **[ENOTRECOVERABLE]** 互斥锁保护的状态不可恢复。
- **[EOWNERDEAD]** 互斥锁是健壮互斥锁，且包含先前拥有线程的进程在持有互斥锁时终止。互斥锁应由调用线程获取，由新所有者负责使状态一致。

`pthread_mutex_lock()` 函数应在以下情况下失败：

- **[EDEADLK]** 互斥锁类型为 `PTHREAD_MUTEX_ERRORCHECK` 且当前线程已拥有互斥锁。

`pthread_mutex_trylock()` 函数应在以下情况下失败：

- **[EBUSY]** 无法获取 `mutex`，因为它已被锁定。

`pthread_mutex_unlock()` 函数应在以下情况下失败：

- **[EPERM]** 互斥锁类型为 `PTHREAD_MUTEX_ERRORCHECK` 或 `PTHREAD_MUTEX_RECURSIVE`，或互斥锁是健壮互斥锁，且当前线程不拥有互斥锁。

`pthread_mutex_lock()` 和 `pthread_mutex_trylock()` 函数可能在以下情况下失败：

- **[EOWNERDEAD]** 互斥锁是健壮互斥锁，且先前的拥有线程在持有互斥锁时终止。互斥锁应由调用线程获取，由新所有者负责使状态一致。

`pthread_mutex_lock()` 函数可能在以下情况下失败：

- **[EDEADLK]** 检测到死锁条件。

这些函数不应返回 `[EINTR]` 错误代码。

## EXAMPLES

无。

## APPLICATION USAGE

已假设非零返回值是错误的应用程序将需要更新以与健壮互斥锁一起使用，因为线程获取保护当前不一致状态的互斥锁的有效返回是 `[EOWNERDEAD]`。由于排除了此类错误出现的可能性而不检查错误返回的应用程序不应使用健壮互斥锁。如果应用程序应该与普通和健壮互斥锁一起工作，它应检查所有返回值的错误条件，并在必要时采取适当行动。

## RATIONALE

互斥锁对象旨在作为构建其他线程同步函数的低级原语。因此，互斥锁的实现应尽可能高效，这对接口可用的功能产生影响。

互斥锁函数和互斥锁属性的特定默认设置是出于不排除互斥锁锁定和解锁的快速内联实现的愿望。

由于大多数属性只需要在线程将被阻塞时检查，属性的使用不会减慢（常见的）互斥锁锁定情况。

同样，虽然能够提取互斥锁拥有者的线程 ID 可能是可取的，但这需要在每个互斥锁被锁定时存储当前线程 ID，这可能导致不可接受的开销级别。类似的论证适用于 `mutex_tryunlock` 操作。

有关扩展互斥锁类型的进一步基本原理，请参阅 XRAT [线程扩展](https://pubs.opengroup.org/onlinepubs/9799919799/xrat/V4_xsh_chap01.html#tag_22_02_09_08)。

如果实现检测到 `mutex` 参数指定的值不引用已初始化的互斥锁对象，建议函数应失败并报告 `[EINVAL]` 错误。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `pthread_mutex_clocklock()`
- `pthread_mutex_consistent()`
- `pthread_mutex_destroy()`
- `pthread_mutexattr_getrobust()`
- XBD [4.15.2 内存同步](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_15_02)
- [<pthread.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

### Issue 6
`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 函数被标记为线程选项的一部分。

POSIX 实现的以下新要求源于与单一 UNIX 规范的对齐：
- 定义了尝试重新锁定互斥锁时的行为。

为与 IEEE Std 1003.1d-1999 对齐，在 SEE ALSO 部分添加了 `pthread_mutex_timedlock()` 函数。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/98，更新 ERRORS 部分，使 `[EDEADLK]` 错误包括死锁条件的检测。RATIONALE 部分也被重新措辞，以考虑不符合 XSI 的系统。

### Issue 7
应用 SD5-XSH-ERN-43，将 `[EINVAL]` 错误的"应失败"情况标记为依赖于线程优先级保护选项。

从 The Open Group Technical Standard, 2006, Extended API Set Part 3 进行了更改。

`pthread_mutex_lock()`、`pthread_mutex_trylock()` 和 `pthread_mutex_unlock()` 函数从线程选项移动到基础。

以下扩展互斥锁类型从 XSI 选项移动到基础：
- `PTHREAD_MUTEX_NORMAL`
- `PTHREAD_MUTEX_ERRORCHECK`
- `PTHREAD_MUTEX_RECURSIVE`
- `PTHREAD_MUTEX_DEFAULT`

更新 DESCRIPTION 以阐明当 `mutex` 不引用已初始化互斥锁时的行为。

更新 ERRORS 部分以正确考虑所有各种互斥锁类型。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0461 [121], XSH/TC1-2008/0462 [92,428], 和 XSH/TC1-2008/0463 [121]。

### Issue 8
应用 Austin Group Defect 354，添加超出可用于拥有的健壮互斥锁的系统资源的 `[EAGAIN]` 错误。

应用 Austin Group Defect 1115，将"该线程"更改为"调用线程"。