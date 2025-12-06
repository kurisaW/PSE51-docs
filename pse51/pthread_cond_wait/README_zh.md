# pthread_cond_clockwait, pthread_cond_timedwait, pthread_cond_wait — 等待条件变量

## SYNOPSIS

```c
#include <pthread.h>

int pthread_cond_clockwait(pthread_cond_t *restrict cond,
                          pthread_mutex_t *restrict mutex,
                          clockid_t clock_id,
                          const struct timespec *restrict abstime);

int pthread_cond_timedwait(pthread_cond_t *restrict cond,
                          pthread_mutex_t *restrict mutex,
                          const struct timespec *restrict abstime);

int pthread_cond_wait(pthread_cond_t *restrict cond,
                     pthread_mutex_t *restrict mutex);
```

## DESCRIPTION

`pthread_cond_clockwait()`、`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数应该在条件变量上阻塞。应用程序应确保这些函数在调用线程锁定 `mutex` 的情况下被调用；否则，会导致错误（对于 PTHREAD_MUTEX_ERRORCHECK 和健壮互斥锁）或未定义行为（对于其他互斥锁）。

这些函数原子性地释放 `mutex` 并导致调用线程在条件变量 `cond` 上阻塞；这里的"原子性"是指"相对于其他线程对互斥锁然后是条件变量的访问而言是原子的"。也就是说，如果另一个线程能够在即将阻塞的线程释放互斥锁后获取它，那么该线程中随后调用 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 的行为应该就像在即将阻塞的线程阻塞后发出一样。

成功返回时，互斥锁应该已被锁定并且应由调用线程拥有。

如果 `mutex` 是一个健壮互斥锁，其中持有锁的所有者终止且状态可恢复，即使函数返回 [EOWNERDEAD]，互斥锁也应该被获取。

使用条件变量时，总是有一个涉及共享变量的布尔谓词与每个条件等待相关联，如果线程应该继续执行，则该谓词为真。从 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 函数可能发生虚假唤醒。由于从这些函数返回并不意味着该谓词值的任何信息，因此在这样的返回时应该重新评估谓词。

当线程在条件变量上等待时，已为 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 操作指定了特定的互斥锁，在该互斥锁和条件变量之间形成动态绑定，只要至少有一个线程在条件变量上阻塞，该绑定就保持有效。在此期间，任何线程尝试使用不同的互斥锁在该条件变量上等待的效果是未定义的。一旦所有等待线程都被解除阻塞（如通过 `pthread_cond_broadcast()` 操作），该条件变量上的下一个等待操作应与该等待操作指定的互斥锁形成新的动态绑定。即使在线程从条件变量等待中被解除阻塞到它返回给调用者或开始取消清理之间的时间内，条件变量和互斥锁之间的动态绑定可能被移除或替换，被解除阻塞的线程应始终重新获取在其返回的条件等待操作调用中指定的互斥锁。

条件等待（无论是否定时）是一个取消点。当线程的可取消性类型设置为 PTHREAD_CANCEL_DEFERRED 时，在条件等待期间处理取消请求的一个副作用是在调用第一个取消清理处理程序之前互斥锁被（实际上）重新获取。效果就像线程被解除阻塞，允许执行到从 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用返回的点上，但在该点注意到取消请求，并且不是返回给调用者，而是开始线程取消活动，这包括调用取消清理处理程序。

因为被取消而在调用 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 时被阻塞的线程已被解除阻塞，如果有其他线程在该条件变量上阻塞，则该线程不应消耗可能同时指向该条件变量的任何条件信号。

`pthread_cond_clockwait()` 函数应等效于 `pthread_cond_wait()`，除了如果以 `clock_id` 指示的时钟测量的 `abstime` 指定的绝对时间在条件 `cond` 被发信号或广播之前过去（即，该时钟测量的当前时间等于或超过 `abstime`），或者在调用时 `abstime` 指定的绝对时间已经过去，则返回错误。实现应支持将 CLOCK_REALTIME 和 CLOCK_MONOTONIC 作为 `clock_id` 参数传递给 `pthread_cond_clockwait()`。当发生此类超时时，`pthread_cond_clockwait()` 仍会释放并重新获取 `mutex` 引用的互斥锁，并且可能消耗同时指向该条件变量的条件信号。

`pthread_cond_timedwait()` 函数应等效于 `pthread_cond_clockwait()`，除了它缺少 `clock_id` 参数。测量 `abstime` 的时钟应来自条件变量的时钟属性，该属性可以在条件变量创建之前通过 `pthread_condattr_setclock()` 设置。如果未设置时钟属性，默认应为 CLOCK_REALTIME。

如果信号被传递给正在等待条件变量的线程，从信号处理程序返回时，线程恢复等待条件变量，就像它没有被中断一样，或者它应由于虚假唤醒而返回零。

如果这些函数的 `cond` 或 `mutex` 参��指定的值分别未引用已初始化的条件变量或已初始化的互斥锁对象，则行为未定义。

## RETURN VALUE

除了 [ETIMEDOUT]、[ENOTRECOVERABLE] 和 [EOWNERDEAD] 外，所有这些错误检查应表现为在函数处理开始时立即执行，并且应导致错误返回，实际上在修改 `mutex` 指定的互斥锁或 `cond` 指定的条件变量状态之前。

成功完成时，应返回零值；否则，应返回错误号以指示错误。

## ERRORS

这些函数应在以下情况失败：

- **[EAGAIN]**
  - 互斥锁是健壮互斥锁，并且可用的健壮互斥锁拥有的系统资源将被超出。

- **[ENOTRECOVERABLE]**
  - 互斥锁保护的状态不可恢复。

- **[EOWNERDEAD]**
  - 互斥锁是健壮互斥锁，包含先前拥有线程的进程在持有互斥锁时终止。互斥锁应由调用线程获取，并且由新所有者负责使状态保持一致。

- **[EPERM]**
  - 互斥锁类型是 PTHREAD_MUTEX_ERRORCHECK 或互斥锁是健壮互斥锁，并且当前线程不拥有互斥锁。

`pthread_cond_clockwait()` 和 `pthread_cond_timedwait()` 函数应在以下情况失败：

- **[ETIMEDOUT]**
  - `abstime` 指定的时间已过去。

- **[EINVAL]**
  - `abstime` 参数指定了小于零或大于等于 1000 百万的纳秒值，或者传递给 `pthread_cond_clockwait()` 的 `clock_id` 参数无效或不受支持。

这些函数可能在以下情况失败：

- **[EOWNERDEAD]**
  - 互斥锁是健壮互斥锁，先前的拥有线程在持有互斥锁时终止。互斥锁应由调用线程获取，并且由新所有者负责使状态保持一致。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES

无。

## APPLICATION USAGE

已假定非零返回值是错误的应用程序需要更新以与健壮互斥锁一起使用，因为线程获取保护当前不一致状态的互斥锁的有效返回是 [EOWNERDEAD]。由于排除了此类错误出现的可能性而不检查错误返回的应用程序不应使用健壮互斥锁。如果应用程序应该与普通互斥锁和健壮互斥锁一起工作，它应该检查所有返回值的错误条件，并在必要时采取适当的行动。

## RATIONALE

如果实现检测到 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 的 `cond` 参数值未引用已初始化的条件变量，或者检测到 `mutex` 参数值未引用已初始化的互斥锁对象，建议函数应该失败并报告 [EINVAL] 错误。

### Condition Wait Semantics

需要注意的是，当 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 无错误返回时，相关的谓词可能仍然为假。类似地，当 `pthread_cond_clockwait()` 或 `pthread_cond_timedwait()` 因超时错误返回时，由于超时过期和谓词状态变化之间不可避免的竞争，相关的谓词可能为真。

应用程序需要在任何返回时重新检查谓词，因为它不能确定是否有另一个线程正在等待该线程来处理信号，如果没有，则信号会丢失。应用程序有责任检查谓词。

一些实现，特别是在多处理器上，当条件变量在不同处理器上同时发信号时，有时可能导致多个线程被唤醒。

一般来说，每当条件等待返回时，线程必须重新评估与条件等待相关的谓词，以确定它是否可以安全地继续，应该再次等待，还是应该声明超时。从等待返回并不意味着相关谓词为真或为假。

因此，建议将条件等待包含在检查谓词的等效"while 循环"中。

### Timed Wait Semantics

为超时参数指定绝对时间测量有两个原因。首先，相对时间测量可以在指定绝对时间的函数上轻松实现，但在指定相对超时的函数上指定绝对超时存在竞争条件。例如，假设 `clock_gettime()` 返回当前时间，`cond_relative_timed_wait()` 使用相对超时：

```c
clock_gettime(CLOCK_REALTIME, &now)
reltime = sleep_til_this_absolute_time - now;
cond_relative_timed_wait(c, m, &reltime);
```

如果线程在第一条语句和最后一条语句之间被抢占，线程会阻塞太长时间。但是，如果使用绝对超时，阻塞是无关紧要的。如果绝对超时在循环中多次使用，例如包围条件等待的循环，也不需要重新计算。

对于系统时钟被操作员不连续提前的情况，预期实现处理任何在中间时间到期的定时等待，就像该时间实际发生一样。

### Choice of Clock

在带超时等待时应注意决定哪个时钟最合适。默认情况下 `pthread_cond_timedwait()` 使用的系统时钟 CLOCK_REALTIME 可能为了与实际时间校正而向前和向后跳跃。CLOCK_MONOTONIC 保证不会向后跳跃，并且也必须在实时中前进，因此通过 `pthread_cond_clockwait()` 或 `pthread_condattr_setclock()` 使用它可能更合适。

### Cancellation and Condition Wait

条件等待，无论是否定时，都是一个取消点。也就是说，`pthread_cond_clockwait()`、`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数是注意到待处理（或并发）取消请求的点。这样做的原因是在这些点上可能发生无限期等待——无论正在等待什么事件，即使程序完全正确，也可能永远不会发生；例如，正在等待的一些输入数据可能永远不会被发送。通过使条件等待成为取消点，即使线程可能陷入某种无限期等待，也可以被取消并执行其取消清理处理程序。

当线程在条件变量上被阻塞时处理取消请求的副作用是在调用任何取消清理处理程序之前重新获取互斥锁。这样做是为了确保取消清理处理程序在与位于条件等待函数调用之前和之后的关键代码相同的状态中执行。在从 Ada 或 C++ 等语言与 POSIX 线程接口时，也需要此规则，这些语言可能选择将取消映射到语言异常；此规则确保保护关键部分的每个异常处理程序始终可以安全地依赖于相关互斥锁已被锁定的事实，无论异常在关键部分内的确切位置被引发。没有此规则，异常处理程序就没有统一的规则可以遵循关于锁定的规则，因此编码会变得非常繁琐。

因此，由于必须在取消在等待期间传递时关于锁的状态做出某些声明，因此选择了使应用程序编码最方便和无错误的定义。

当线程在条件变量上被阻塞时处理取消请求，实现必须确保如果有其他线程在该条件变量上等待，线程不消耗指向该条件变量的任何条件信号。指定此规则是为了避免如果这两个独立请求（一个作用于线程，另一个作用于条件变量）没有独立处理时可能发生的死锁条件。

### Performance of Mutexes and Condition Variables

互斥锁预期只锁定几条指令。这种做法几乎被程序员避免长串行执行区域的愿望自动强制执行（这会减少总的有效并行性）。

使用互斥锁和条件变量时，人们试图确保通常的情况是锁定互斥锁、访问共享数据并解锁互斥锁。在条件变量上等待应该是相对罕见的情况。例如，当实现读写锁时，获取读锁的代码通常只需要在互斥下增加读取器计数并返回。调用线程只会在已经有活动写入器时才实际在条件变量上等待。因此，同步操作的效率受互斥锁锁定/解锁的成本限制，而不是受条件等待的限制。注意，在通常情况下没有上下文切换。

这并不是说条件等待的效率不重要。由于每次 Ada 会合至少需要一个上下文切换，在条件变量上等待的效率是重要的。在条件变量上等待的成本应该略多于上下文切换的最小成本加上解锁和锁定互斥锁的时间。

### Features of Mutexes and Condition Variables

有人建议将互斥锁获取和释放与条件等待解耦。这被拒绝，因为正是操作的���合性质实际上促进了实时实现。这些实现可以透明地将高优先级线程在条件变量和互斥锁之间原子性地移动，这对调用者是透明的。这可以防止额外的上下文切换，并在等待线程被发信号时提供更确定性的互斥锁获取。因此，公平性和优先级问题可以通过调度规则直接处理。此外，当前的条件等待操作符合现有实践。

### Scheduling Behavior of Mutexes and Condition Variables

试图通过指定排序规则干扰调度策略的同步原语被认为是不受欢迎的。在互斥锁和条件变量上等待的线程被选择继续执行的顺序依赖于调度策略，而不是某个固定顺序（例如，FIFO 或优先级）。因此，调度策略决定哪些线程被唤醒并允许继续。

### Timed Condition Wait

`pthread_cond_clockwait()` 和 `pthread_cond_timedwait()` 函数允许应用程序在给定时间后放弃等待特定条件。示例如下：

```c
(void) pthread_mutex_lock(&t.mn);
    t.waiters++;
    clock_gettime(CLOCK_MONOTONIC, &ts);
    ts.tv_sec += 5;
    rc = 0;
    while (! mypredicate(&t) && rc == 0)
        rc = pthread_cond_clockwait(&t.cond, &t.mn,
            CLOCK_MONOTONIC, &ts);
    t.waiters--;
    if (rc == 0 || mypredicate(&t))
        setmystate(&t);
(void) pthread_mutex_unlock(&t.mn);
```

通过使超时参数绝对，程序每次检查其阻塞谓词时都不需要重新计算它。如果超时是相对的，就必须在每次调用之前重新计算。这将是特别困难的，因为这样的代码需要考虑在谓词为真或超时到期之前发生的额外广播或条件变量信号导致的额外唤醒的可能性。使用 CLOCK_MONOTONIC 而不是 CLOCK_REALTIME 意味着超时不受系统时钟更改的影响。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `pthread_cond_broadcast()`
- **XBD** 4.15.2 Memory Synchronization, `<pthread.h>`

## CHANGE HISTORY

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含。

### Issue 6

`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数被标记为 Threads 选项的一部分。

应用 The Open Group Corrigendum U021/9，更正了 `pthread_cond_wait()` 函数的原型。

DESCRIPTION 通过添加 Clock Selection 选项的语义更新，以与 IEEE Std 1003.1j-2000 对齐。

ERRORS 部分有额外情况

---
