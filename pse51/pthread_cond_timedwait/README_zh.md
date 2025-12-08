# pthread_cond_clockwait, pthread_cond_timedwait, pthread_cond_wait — 等待条件变量

## SYNOPSIS（概要）

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

## DESCRIPTION（描述）

`pthread_cond_clockwait()`、`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数应该在条件变量上阻塞。应用程序应确保这些函数在调用线程锁定 `mutex` 的情况下被调用；否则，会导致错误（对于 PTHREAD_MUTEX_ERRORCHECK 和健壮互斥锁）或未定义行为（对于其他互斥锁）。

这些函数原子地释放 `mutex` 并使调用线程在条件变量 `cond` 上阻塞；这里的"原子地"是指"相对于另一个线程对互斥锁然后对条件变量的访问是原子的"。也就是说，如果另一个线程在即将阻塞的线程释放互斥锁后能够获取该互斥锁，那么该线程中后续的 `pthread_cond_broadcast()` 或 `pthread_cond_signal()` 调用的行为应等同于在即将阻塞的线程已经阻塞之后发出。

成功返回时，互斥锁应该已被锁定并由调用线程拥有。

如果 `mutex` 是一个健壮互斥锁，其持有者在持有锁时终止且状态可恢复，那么即使函数返回 [EOWNERDEAD]，互斥锁也应被获取。

使用条件变量时，总是有一个与每个条件等待相关的涉及共享变量的布尔谓词，如果线程应该继续执行，该谓词为真。`pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 函数可能会发生虚假唤醒。由于从这些函数返回并不意味着该谓词的值有任何意义，因此在这样的返回时应重新评估谓词。

当线程在条件变量上等待时，已为 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 操作指定了特定的互斥锁，在该互斥锁和条件变量之间形成了一个动态绑定，只要至少有一个线程在该条件变量上阻塞，该绑定就保持有效。在此期间，任何线程尝试使用不同的互斥锁在该条件变量上等待的效果是未定义的。一旦所有等待的线程都被解除阻塞（如通过 `pthread_cond_broadcast()` 操作），该条件变量上的下一个等待操作应与该等待操作指定的互斥锁形成新的动态绑定。尽管在线程从条件变量等待中被解除阻塞到返回给调用者或开始取消清理之间的时间内，条件变量和互斥锁之间的动态绑定可能被移除或替换，但被解除阻塞的线程应始终重新获取在条件等待操作调用中指定的互斥锁。

条件等待（无论是否超时）是一个取消点。当线程的可取消性类型设置为 PTHREAD_CANCEL_DEFERRED 时，在条件等待期间处理取消请求的一个副作用是互斥锁在调用第一个取消清理处理程序之前（实际上）被重新获取。效果如同线程被解除阻塞，允许执行到从 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用返回的点，但在该点注意到取消请求，而不是返回给调用者，而是开始线程取消活动，包括调用取消清理处理程序。

在调用 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 时被阻塞而被取消的线程，如果有其他线程在该条件变量上阻塞，则不应消耗可能同时指向该条件变量的任何条件信号。

`pthread_cond_clockwait()` 函数应等同于 `pthread_cond_wait()`，不同的是，如果以 `clock_id` 指示的时钟测量的 `abstime` 指定的绝对时间已过（即，该时钟测量的当前时间等于或超过 `abstime`）在条件 `cond` 被信号或广播之前，或者在调用时 `abstime` 指定的绝对时间已经过去，则返回错误。实现应支持将 CLOCK_REALTIME 和 CLOCK_MONOTONIC 作为 `clock_id` 参数传递给 `pthread_cond_clockwait()`。当发生此类超时时，`pthread_cond_clockwait()` 仍会释放并重新获取 `mutex` 引用的互斥锁，并且可能消耗指向该条件变量的条件信号。

`pthread_cond_timedwait()` 函数应等同于 `pthread_cond_clockwait()`，但它缺少 `clock_id` 参数。用于测量 `abstime` 的时钟应来自条件变量的时钟属性，该属性可以在条件变量创建之前通过 `pthread_condattr_setclock()` 设置。如果未设置时钟属性，默认应为 CLOCK_REALTIME。

如果信号被传递给正在等待条件变量的线程，从信号处理程序返回后，线程将恢复等待条件变量，如同未被中断一样，或者由于虚假唤醒而返回零。

如果传递给这些函数的 `cond` 或 `mutex` 参数的值分别没有引用已初始化的条件变量或已初始化的互斥锁对象，则行为未定义。

## RETURN VALUE（返回值）

除了 [ETIMEDOUT]、[ENOTRECOVERABLE] 和 [EOWNERDEAD] 外，所有这些错误检查都应如同在函数处理开始时立即执行一样，并且应导致错误返回，实际上是在修改 `mutex` 指定的互斥锁或 `cond` 指定的条件变量的状态之前。

成功完成时，应返回零值；否则，应返回错误编号以指示错误。

## ERRORS（错误）

这些函数可能失败，如果：

- **[EAGAIN]**
  互斥锁是健壮互斥锁，并且系统可用的健壮互斥锁资源将被超出。

- **[ENOTRECOVERABLE]**
  互斥锁保护的状态不可恢复。

- **[EOWNERDEAD]**
  互斥锁是健壮互斥锁，包含先前拥有线程的进程在持有互斥锁时终止。互斥锁应由调用线程获取，由新所有者负责使状态一致。

- **[EPERM]**
  互斥锁类型是 PTHREAD_MUTEX_ERRORCHECK 或互斥锁是健壮互斥锁，且当前线程不拥有互斥锁。

`pthread_cond_clockwait()` 和 `pthread_cond_timedwait()` 函数可能失败，如果：

- **[ETIMEDOUT]**
  `abstime` 指定的时间已经过去。

- **[EINVAL]**
  `abstime` 参数指定的纳秒值小于零或大于等于 1000 百万，或者传递给 `pthread_cond_clockwait()` 的 `clock_id` 参数无效或不受支持。

这些函数可能失败，如果：

- **[EOWNERDEAD]**
  互斥锁是健壮互斥锁，先前的拥有线程在持有互斥锁时终止。互斥锁应由调用线程获取，由新所有者负责使状态一致。

这些函数不应返回 [EINTR] 错误代码。

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

假设非零返回值为错误的应用程序需要更新以使用健壮互斥锁，因为线程获取保护当前不一致状态的互斥锁的有效返回是 [EOWNERDEAD]。不检查错误返回的应用程序（由于排除了此类错误出现的可能性）不应使用健壮互斥锁。如果应用程序应该与普通和健壮互斥锁一起工作，它应该检查所有返回值的错误条件，并在必要时采取适当措施。

## RATIONALE（原理）

如果实现检测到传递给 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 的 `cond` 参数值没有引用已初始化的条件变量，或者检测到 `mutex` 参数值没有引用已初始化的互斥锁对象，建议函数应该失败并报告 [EINVAL] 错误。

### Condition Wait Semantics（条件等待语义）

需要注意的是，当 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 无错误返回时，相关的谓词可能仍然为假。类似地，当 `pthread_cond_clockwait()` 或 `pthread_cond_timedwait()` 因超时错误返回时，相关的谓词可能为真，这是由于超时到期和谓词状态变化之间不可避免的竞争条件。

应用程序需要在任何返回时重新检查谓词，因为它不能确定是否有另一个线程在等待线程来处理信号，如果没有，那么信号就丢失了。检查谓词的责任在于应用程序。

一些实现，特别是在多处理器上，有时可能在条件变量在不同处理器上同时被信号时导致多个线程被唤醒。

一般来说，无论条件等待何时返回，线程都必须重新评估与条件等待相关的谓词，以确定它是否可以安全继续、应该再次等待，还是应该声明超时。从等待返回并不意味着相关的谓词要么为真要么为假。

因此，建议条件等待包含在检查谓词的等效"while 循环"中。

### Timed Wait Semantics（超时等待语义）

为超时参数指定绝对时间度量的原因有两个。首先，相对时间度量可以在指定绝对时间的函数之上轻松实现，但在指定相对超时的函数之上指定绝对超时存在竞争条件。例如，假设 `clock_gettime()` 返回当前时间，`cond_relative_timed_wait()` 使用相对超时：

```c
clock_gettime(CLOCK_REALTIME, &now)
reltime = sleep_til_this_absolute_time -now;
cond_relative_timed_wait(c, m, &reltime);
```

如果线程在第一个语句和最后一个语句之间被抢占，线程将阻塞太久。然而，如果使用绝对超时，阻塞是无关紧要的。如果在循环中多次使用绝对超时，也不需要重新计算。

对于操作员不连续地提前系统时钟的情况，预期实现会处理在中间时间到期的任何超时等待，如同该时间实际发生一样。

### Choice of Clock（时钟选择）

在超时等待时，应谨慎决定哪个时钟最合适。`pthread_cond_timedwait()` 默认使用的系统时钟 CLOCK_REALTIME 可能为了与实际时间校正而向前和向后跳变。CLOCK_MONOTONIC 保证不会向后跳变，并且也必须在实时中前进，因此通过 `pthread_cond_clockwait()` 或 `pthread_condattr_setclock()` 使用它可能更合适。

### Cancellation and Condition Wait（取消和条件等待）

条件等待，无论是否超时，都是一个取消点。也就是说，`pthread_cond_clockwait()`、`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数是注意到挂起（或并发）取消请求的点。这样做的原因是在这些点可能发生无限期等待——无论等待什么事件，即使程序完全正确，也可能永远不会发生；例如，等待的某些输入数据可能永远不会被发送。通过使条件等待成为取消点，线程可以被取消并执行其取消清理处理程序，即使它可能被卡在某个无限期等待中。

在条件变量上阻塞时处理取消请求的一个副作用是在调用任何取消清理处理程序之前重新获取互斥锁。这样做是为了确保取消清理处理程序在与条件等待函数调用之前和之后的关键代码相同的状态中执行。在从 Ada 或 C++ 等语言与 POSIX 线程接口时，也需要这个规则，这些语言可能选择将取消映射到语言异常；这个规则确保每个保护关键段的异常处理程序总是可以安全地依赖于相关互斥锁已被锁定的事实，无论异常在关键段内的确切位置被引发。没有这个规则，异常处理程序就没有统一的规则来遵循关于锁的行为，因此编码将变得非常繁琐。

因此，由于必须就取消在等待期间传递时锁的状态做出某些声明，选择了使应用程序编码最方便和无错误的定义。

在线程在条件变量上阻塞时处理取消请求时，实现需要确保如果有其他线程在该条件变量上等待，线程不会消耗任何指向该条件变量的条件信号。指定这个规则是为了避免如果这两个独立请求（一个作用于线程，另一个作用于条件变量）不被独立处理时可能发生的死锁条件。

### Performance of Mutexes and Condition Variables（互斥锁和条件变量的性能）

互斥锁预期只被锁定几条指令。这种做法几乎被程序员避免长串行执行区域（这将减少总的有效并行性）的愿望自动强制执行。

使用互斥锁和条件变量时，人们试图确保通常的情况是锁定互斥锁、访问共享数据、解锁互斥锁。在条件变量上等待应该是一个相对罕见的情况。例如，在实现读写锁时，获取读锁的代码通常只需要（在互斥下）增加读取器计数并返回。只有在已经有活动写入器时，调用线程才会在条件变量上实际等待。所以同步操作的效率以互斥锁锁定/解锁的成本为界限，而不是以条件等待为界限。注意，在通常情况下没有上下文切换。

这并不是说条件等待的效率不重要。由于每次 Ada 会合至少需要一次上下文切换，在条件变量上等待的效率很重要。在条件变量上等待的成本应该略多于上下文切换的最小成本加上解锁和锁定互斥锁的时间。

### Features of Mutexes and Condition Variables（互斥锁和条件变量的特性）

有人建议将互斥锁的获取和释放与条件等待解耦。这被拒绝了，因为操作的组合性质实际上促进了实时实现。这些实现可以以对调用者透明的方式原子地在条件变量和互斥锁之间移动高优先级线程。这可以防止额外的上下文切换，并在等待线程被信号时提供更确定的互斥锁获取。因此，公平性和优先级问题可以通过调度规则直接处理。此外，当前的条件等待操作与现有实践相匹配。

### Scheduling Behavior of Mutexes and Condition Variables（互斥锁和条件变量的调度行为）

试图通过指定排序规则来干扰调度策略的同步原语被认为是不受欢迎的。等待互斥锁和条件变量的线程被选择继续的顺序取决于调度策略，而不是某个固定顺序（例如，FIFO 或优先级）。因此，调度策略决定哪些线程被唤醒并允许继续。

### Timed Condition Wait（超时条件等待）

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

通过使超时参数为绝对值，不需要在程序每次检查其阻塞谓词时重新计算。如果超时是相对的，则必须在每次调用之前重新计算。这将特别困难，因为这样的代码需要考虑由于在谓词为真或超时到期之前发生的额外广播或条件变量信号导致的额外唤醒的可能性。使用 CLOCK_MONOTONIC 而不是 CLOCK_REALTIME 意味着超时不受系统时钟更改的影响。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（另请参见）

- `pthread_cond_broadcast()`
- XBD 4.15.2 Memory Synchronization, `<pthread.h>`

## CHANGE HISTORY（变更历史）

首次发布于 Issue 5。包含用于与 POSIX 线程扩展对齐。

### Issue 6

`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数被标记为 Threads 选项的一部分。

应用了 The Open Group Corrigendum U021/9，更正了 `pthread_cond_wait()` 函数的原型。

DESCRIPTION 通过添加 Clock Selection 选项的语义进行更新，以与 IEEE Std 1003.1j-2000 对齐。

响应 IEEE PASC Interpretation 1003.1c #28，ERRORS 部分为 [EPERM] 添加了额外情况。

为与 ISO/IEC 9899:1999 标准对齐，向 `pthread_cond_timedwait()` 和 `pthread_cond_wait()` 原型添加了 `restrict` 关键字。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/89，更新 DESCRIPTION 以与 `pthread_cond_destroy()` 函数保持一致，该函数声明销毁没有线程当前阻塞的已初始化条件变量是安全的。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/90，将 DESCRIPTION 中的措辞从"the cancelability enable state"更新为"the cancelability type"。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/91，更新 ERRORS 部分，从 `pthread_cond_wait()` 函数中移除与 `abstime` 相关的错误情况，并使与 `abstime` 相关的错误情况对 `pthread_cond_timedwait()` 强制执行，以与其他函数保持一致。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/92，向 RATIONALE 部分添加新段落，说明应用程序应检查此函数的任何返回时的谓词。

### Issue 7

应用了 SD5-XSH-ERN-44，更改了 [EINVAL] 错误的"shall fail"情况的定义。

从 The Open Group Technical Standard, 2006, Extended API Set Part 3 进行了更改。

`pthread_cond_timedwait()` 和 `pthread_cond_wait()` 函数从 Threads 选项移动到 Base。

移除了未初始化条件变量或未初始化互斥锁对象的 [EINVAL] 错误；这种情况导致未定义行为。

[EPERM] 错误被修订并移动到 `pthread_cond_timedwait()` 函数的错误条件的"shall fail"列表。

更新 DESCRIPTION 以阐明当 `mutex` 是健壮互斥锁时的行为。

更新 ERRORS 部分以包含 PTHREAD_MUTEX_ERRORCHECK 互斥锁的"shall fail"情况。

重写 DESCRIPTION 以阐明未定义行为仅发生在未强制要求 [EPERM] 错误的互斥锁上。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0456 [91,286,437] 和 XSH/TC1-2008/0457 [239]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0271 [749]。

### Issue 8

应用了 Austin Group Defect 354，添加了 [EAGAIN] 错误。

应用了 Austin Group Defect 1162，将"an error code"更改为"[EOWNERDEAD]"。

应用了 Austin Group Defects 1216 和 1485，添加了 `pthread_cond_clockwait()`。

---
