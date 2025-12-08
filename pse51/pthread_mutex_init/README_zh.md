# pthread_mutex_destroy, pthread_mutex_init

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutex_destroy(pthread_mutex_t *mutex);
int pthread_mutex_init(pthread_mutex_t *restrict mutex,
                      const pthread_mutexattr_t *restrict attr);
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

## DESCRIPTION

`pthread_mutex_destroy()` 函数应当销毁由 `mutex` 引用的互斥锁对象；该互斥锁对象实际上变为未初始化状态。实现可以使 `pthread_mutex_destroy()` 将由 `mutex` 引用的对象设置为无效值。

已销毁的互斥锁对象可以使用 `pthread_mutex_init()` 重新初始化；在销毁后其他方式引用该对象的结果是未定义的。

销毁一个已初始化且未锁定的互斥锁是安全的。尝试销毁一个已锁定的互斥锁，或另一个线程正在尝试锁定的互斥锁，或正在被另一个线程在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中使用的互斥锁，会导致未定义行为。

`pthread_mutex_init()` 函数应当使用由 `attr` 指定的属性初始化由 `mutex` 引用的互斥锁。如果 `attr` 为 NULL，则使用默认的互斥锁属性；其效果应与传递默认互斥锁属性对象的地址相同。成功初始化后，互斥锁的状态变为已初始化且未锁定。

进一步要求请参见 [2.9.9 同步对象副本和替代映射](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_09_09)。

尝试初始化一个已初始化的互斥锁会导致未定义行为。

在默认互斥锁属性合适的情况下，可以使用宏 PTHREAD_MUTEX_INITIALIZER 来初始化互斥锁。其效果应等同于通过调用 `pthread_mutex_init()` 并将参数 `attr` 指定为 NULL 进行动态初始化，但不执行错误检查。

如果传递给 `pthread_mutex_destroy()` 的 `mutex` 参数值不引用已初始化的互斥锁，则行为未定义。

如果传递给 `pthread_mutex_init()` 的 `attr` 参数值不引用已初始化的互斥锁属性对象，则行为未定义。

## RETURN VALUE

如果成功，`pthread_mutex_destroy()` 和 `pthread_mutex_init()` 函数应返回零；否则，应返回错误码以指示错误。

## ERRORS

`pthread_mutex_init()` 函数在以下情况下应失败：

- **[EAGAIN]**: 系统缺乏必要的资源（内存除外）来初始化另一个互斥锁。
- **[ENOMEM]**: 内存不足，无法初始化互斥锁。
- **[EPERM]**: 调用者没有执行该操作的权限。

`pthread_mutex_init()` 函数在以下情况下可能失败：

- **[EINVAL]**: 由 `attr` 引用的属性对象设置了健壮互斥锁属性，但未设置进程共享属性。

这些函数不应返回 [EINTR] 错误码。

---

*以下各节为信息性内容。*

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

如果实现检测到传递给 `pthread_mutex_destroy()` 的 `mutex` 参数值不引用已初始化的互斥锁，建议函数失败并报告 [EINVAL] 错误。

如果实现检测到传递给 `pthread_mutex_destroy()` 或 `pthread_mutex_init()` 的 `mutex` 参数值引用已锁定的互斥锁或被另一个线程引用（例如，在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中使用）的互斥锁，或者检测到传递给 `pthread_mutex_init()` 的 `mutex` 参数值引用已初始化的互斥锁，建议函数失败并报告 [EBUSY] 错误。

如果实现检测到传递给 `pthread_mutex_init()` 的 `attr` 参数值不引用已初始化的互斥锁属性对象，建议函数失败并报告 [EINVAL] 错误。

### 可能的替代实现

本 POSIX.1-2024 卷支持互斥锁的几种替代实现。实现可以将锁直接存储在 `pthread_mutex_t` 类型的对象中。或者，实现可以将锁存储在堆中，并仅在互斥锁对象中存储指针、句柄或唯一 ID。任一实现都有其优势，或者在特定硬件配置下可能是必需的。为了能够编写对这种选择不变的便携代码，本 POSIX.1-2024 卷没有为此类型定义赋值或相等性，并使用术语"初始化"来加强（更具限制性的）概念，即锁实际上可能驻留在互斥锁对象本身中。

请注意，这排除了对互斥锁或条件变量类型的过度规范，并促使该类型的不透明性。

允许但不是要求 `pthread_mutex_destroy()` 在互斥锁中存储非法值。这可能有助于检测试图锁定（或以其他方式引用）已销毁互斥锁的错误程序。

### 支持错误检查与性能之间的权衡

许多可能发生的错误情况不需要实现检测，以便让实现根据其特定应用程序和执行环境的需要，在性能与错误检查程度之间进行权衡。作为一般规则，由系统引起的条件（如内存不足）要求检测，但由错误编码的应用程序引起的条件（如未能提供足够的同步以防止互斥锁在使用时被删除）被指定为导致未定义行为。

因此，可以实现广泛的实现范围。例如，用于应用程序调试的实现可以实现所有错误检查，但在嵌入式计算机上在非常严格的性能约束下运行单个、经证明正确的应用程序的实现可能实现最少的检查。实现甚至可能以两个版本提供，类似于编译器提供的选项：一个全检查但较慢的版本；和一个有限检查但更快的版本。禁止这种可选性对用户来说是一种不友好的做法。

通过仔细将"未定义行为"的使用限制为错误（错误编码的）应用程序可能做的事情，并通过定义资源不可用错误是强制性的，本 POSIX.1-2024 卷确保完全符合规范的应用程序在全部实现范围内是可移植的，同时不强制所有实现为检查正确程序永远不会做的众多事情而添加开销。当行为未定义时，对于确实检测到该条件的实现，没有指定要返回的错误码。这是因为未定义行为意味着任何事情都可能发生，包括返回任何值（这可能恰好是有效但不同的错误码）。但是，由于错误码可能对应用程序开发人员在应用程序开发期间诊断问题时有帮助，在基本原理中提出了一个建议：如果他们的实现确实检测到该条件，实现者应返回特定的错误码。

### 为什么没有定义限制

曾考虑为互斥锁和条件变量的最大数量定义符号，但被拒绝，因为这些对象的数量可能动态变化。此外，许多实现将这些对象放在应用程序内存中；因此，没有明确的限制。

### 互斥锁和条件变量的静态初始化程序

为静态分配的同步对象提供静态初始化允许具有私有静态同步变量的模块避免运行时初始化测试和开销。此外，它简化了自初始化模块的编码。此类模块在 C 库中很常见，其中出于各种原因，设计要求自初始化，而不是要求调用显式的模块初始化函数。静态初始化的示例如下。

没有静态初始化，自初始化例程 `foo()` 可能如下所示：

```c
static pthread_once_t foo_once = PTHREAD_ONCE_INIT;
static pthread_mutex_t foo_mutex;

void foo_init()
{
    pthread_mutex_init(&foo_mutex, NULL);
}

void foo()
{
    pthread_once(&foo_once, foo_init);
    pthread_mutex_lock(&foo_mutex);
    /* 执行工作。 */
    pthread_mutex_unlock(&foo_mutex);
}
```

使用静态初始化，相同的例程可以编码如下：

```c
static pthread_mutex_t foo_mutex = PTHREAD_MUTEX_INITIALIZER;

void foo()
{
    pthread_mutex_lock(&foo_mutex);
    /* 执行工作。 */
    pthread_mutex_unlock(&foo_mutex);
}
```

请注意，静态初始化既消除了在 `pthread_once()` 内部进行初始化测试的需要，也消除了获取 `foo_mutex` 以了解要传递给 `pthread_mutex_lock()` 或 `pthread_mutex_unlock()` 的地址的需要。

因此，在所有系统上为初始化静态对象而编写的 C 代码更简单，并且在大量系统上也更快；那些（整个）同步对象可以存储在应用程序内存中的系统。

然而，对于需要将互斥锁分配出特殊内存的机器，可能会提出锁定性能问题。此类机器实际上必须让互斥锁，可能还有条件变量包含指向实际硬件锁的指针。为了使静态初始化在此类机器上工作，`pthread_mutex_lock()` 还必须测试实际锁的指针是否已分配。如果没有，`pthread_mutex_lock()` 在使用前必须初始化它。此类资源的保留可以在程序加载时进行，因此没有向互斥锁锁定和条件变量等待添加返回码以指示未能完成初始化。

`pthread_mutex_lock()` 中的此运行时测试起初似乎是额外的工作；需要额外的测试来查看指针是否已初始化。在大多数机器上，这实际上将实现为获取指针、将指针与零进行测试，然后在指针已初始化时使用指针。虽然测试似乎会增加额外的工作，但测试寄存器的额外努力通常可以忽略不计，因为实际上没有进行额外的内存引用。随着越来越多的机器提供缓存，真正的开销是内存引用，而不是执行的指令。

或者，根据机器架构，通常有方法可以在最重要的情况下消除所有开销：在已初始化锁之后发生的锁操作上。这可以通过将更多开销转移到较不频繁的操作来实现：初始化。由于互斥锁的线外分配也意味着必须取消引用地址以找到实际的锁，一种广泛适用的技术是让静态初始化为该地址存储一个伪值；特别是，会导致机器故障发生的地址。当在首次尝试锁定此类互斥锁时发生此类故障时，可以进行有效性检查，然后可以填入实际锁的正确地址。后续的锁操作不会产生额外开销，因为它们不会"故障"。这仅仅是可用于支持静态初始化而不对锁获取性能产生不利影响的技术之一。无疑还有其他高度依赖于机器的技术。

因此，对于进行线外互斥锁分配的机器，锁定开销对于隐式初始化的模块是相似的，而对于完全内联进行互斥锁分配的模块则有所改善。内联情况因此变得更快，而线外情况不会明显更差。

除了此类机器的锁定性能问题外，还有一个担忧是线程可能在尝试完成静态分配的互斥锁初始化时为初始化锁序列化竞争。（此类完成通常涉及获取内部锁、分配结构、将指向结构的指针存储在互斥锁中，并释放内部锁。）首先，许多实现会通过对互斥锁地址进行哈希来减少此类序列化。其次，此类序列化只能发生有限的次数。特别是，它最多可能发生与静态分配同步对象数量相同的次数。动态分配的对象仍将通过 `pthread_mutex_init()` 或 `pthread_cond_init()` 初始化。

最后，如果以上线外分配的优化技术都没有为某个实现上的应用程序提供足够的性能，应用程序可以通过使用相应的 `pthread_*_init()` 函数显式初始化所有同步对象来完全避免静态初始化，这些函数被所有实现支持。实现还可以记录权衡并建议哪种初始化技术对于该特定实现更有效。

### 销毁互斥锁

互斥锁可以在解锁后立即销毁。但是，由于尝试销毁已锁定的互斥锁，或另一个线程正在尝试锁定的互斥锁，或正在被另一个线程在 `pthread_cond_clockwait()`、`pthread_cond_timedwait()` 或 `pthread_cond_wait()` 调用中使用的互斥锁，会导致未定义行为，必须注意确保没有其他线程可能在引用该互斥锁。

### 健壮互斥锁

对于进程共享属性设置为 PTHREAD_PROCESS_SHARED 的互斥锁，要求实现提供健壮互斥锁。当进程共享属性设置为 PTHREAD_PROCESS_PRIVATE 时，允许但不是要求实现提供健壮互斥锁。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[`pthread_mutex_getprioceiling()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_getprioceiling.html), [`pthread_mutexattr_getrobust()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getrobust.html), [`pthread_mutex_clocklock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_clocklock.html), [`pthread_mutex_lock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_lock.html), [`pthread_mutexattr_getpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getpshared.html)

XBD [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## CHANGE HISTORY

首次发布于 Issue 5。为与 POSIX 线程扩展对齐而包含在内。

### Issue 6

`pthread_mutex_destroy()` 和 `pthread_mutex_init()` 函数被标记为线程选项的一部分。

为了与 IEEE Std 1003.1-2001 对齐，在 SEE ALSO 部分添加了 `pthread_mutex_timedlock()` 函数。
