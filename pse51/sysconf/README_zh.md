# sysconf

**The Open Group Base Specifications Issue 8**
**IEEE Std 1003.1-2024**
版权所有 © 2001-2024 The IEEE and The Open Group

---

## 名称 (NAME)

sysconf — 获取可配置的系统变量

## 概要 (SYNOPSIS)

```c
#include <unistd.h>

long sysconf(int name);
```

## 描述 (DESCRIPTION)

`sysconf()` 函数为应用程序提供了一种方法来确定可配置系统限制或选项（变量）的当前值。实现必须支持下表中列出的所有变量，并且可以支持其他变量。

`name` 参数表示要查询的系统变量。下表列出了 `sysconf()` 可以返回的来自 `<limits.h>` 或 `<unistd.h>` 的最小系统变量集合，以及在 `<unistd.h>` 中定义的用于 `name` 的相应符号常量值。

### 系统变量 (System Variables)

| 变量 | Name 参数值 |
|----------|---------------|
| {AIO_LISTIO_MAX} | _SC_AIO_LISTIO_MAX |
| {AIO_MAX} | _SC_AIO_MAX |
| {AIO_PRIO_DELTA_MAX} | _SC_AIO_PRIO_DELTA_MAX |
| {ARG_MAX} | _SC_ARG_MAX |
| {ATEXIT_MAX} | _SC_ATEXIT_MAX |
| {BC_BASE_MAX} | _SC_BC_BASE_MAX |
| {BC_DIM_MAX} | _SC_BC_DIM_MAX |
| {BC_SCALE_MAX} | _SC_BC_SCALE_MAX |
| {BC_STRING_MAX} | _SC_BC_STRING_MAX |
| {CHILD_MAX} | _SC_CHILD_MAX |
| 时钟滴答/秒 | _SC_CLK_TCK |
| {COLL_WEIGHTS_MAX} | _SC_COLL_WEIGHTS_MAX |
| {DELAYTIMER_MAX} | _SC_DELAYTIMER_MAX |
| {EXPR_NEST_MAX} | _SC_EXPR_NEST_MAX |
| {HOST_NAME_MAX} | _SC_HOST_NAME_MAX |
| {IOV_MAX} | _SC_IOV_MAX |
| {LINE_MAX} | _SC_LINE_MAX |
| {LOGIN_NAME_MAX} | _SC_LOGIN_NAME_MAX |
| {NGROUPS_MAX} | _SC_NGROUPS_MAX |
| `getgrgid_r()` 和 `getgrnam_r()` 数据缓冲区的初始大小 | _SC_GETGR_R_SIZE_MAX |
| `getpwuid_r()` 和 `getpwnam_r()` 数据缓冲区的初始大小 | _SC_GETPW_R_SIZE_MAX |
| {MQ_OPEN_MAX} | _SC_MQ_OPEN_MAX |
| {MQ_PRIO_MAX} | _SC_MQ_PRIO_MAX |
| 可用于运行线程的最大执行单元数量† | _SC_NPROCESSORS_CONF |
| 当前可用于运行线程的最大执行单元数量† | _SC_NPROCESSORS_ONLN |
| 支持的最高信号编号 +1 | _SC_NSIG |
| {OPEN_MAX} | _SC_OPEN_MAX |
| {PAGE_SIZE} | _SC_PAGE_SIZE |
| {PAGESIZE} | _SC_PAGESIZE |
| {PTHREAD_DESTRUCTOR_ITERATIONS} | _SC_THREAD_DESTRUCTOR_ITERATIONS |
| {PTHREAD_KEYS_MAX} | _SC_THREAD_KEYS_MAX |
| {PTHREAD_STACK_MIN} | _SC_THREAD_STACK_MIN |
| {PTHREAD_THREADS_MAX} | _SC_THREAD_THREADS_MAX |
| {RE_DUP_MAX} | _SC_RE_DUP_MAX |
| {RTSIG_MAX} | _SC_RTSIG_MAX |
| {SEM_NSEMS_MAX} | _SC_SEM_NSEMS_MAX |
| {SEM_VALUE_MAX} | _SC_SEM_VALUE_MAX |
| {SIGQUEUE_MAX} | _SC_SIGQUEUE_MAX |
| {STREAM_MAX} | _SC_STREAM_MAX |
| {SYMLOOP_MAX} | _SC_SYMLOOP_MAX |
| {TIMER_MAX} | _SC_TIMER_MAX |
| {TTY_NAME_MAX} | _SC_TTY_NAME_MAX |
| {TZNAME_MAX} | _SC_TZNAME_MAX |
| _POSIX_ADVISORY_INFO | _SC_ADVISORY_INFO |
| _POSIX_BARRIERS | _SC_BARRIERS |
| _POSIX_ASYNCHRONOUS_IO | _SC_ASYNCHRONOUS_IO |
| _POSIX_CLOCK_SELECTION | _SC_CLOCK_SELECTION |
| _POSIX_CPUTIME | _SC_CPUTIME |
| _POSIX_DEVICE_CONTROL | _SC_DEVICE_CONTROL |
| _POSIX_FSYNC | _SC_FSYNC |
| _POSIX_IPV6 | _SC_IPV6 |
| _POSIX_JOB_CONTROL | _SC_JOB_CONTROL |
| _POSIX_MAPPED_FILES | _SC_MAPPED_FILES |
| _POSIX_MEMLOCK | _SC_MEMLOCK |
| _POSIX_MEMLOCK_RANGE | _SC_MEMLOCK_RANGE |
| _POSIX_MEMORY_PROTECTION | _SC_MEMORY_PROTECTION |
| _POSIX_MESSAGE_PASSING | _SC_MESSAGE_PASSING |
| _POSIX_MONOTONIC_CLOCK | _SC_MONOTONIC_CLOCK |
| _POSIX_PRIORITIZED_IO | _SC_PRIORITIZED_IO |
| _POSIX_PRIORITY_SCHEDULING | _SC_PRIORITY_SCHEDULING |
| _POSIX_RAW_SOCKETS | _SC_RAW_SOCKETS |
| _POSIX_READER_WRITER_LOCKS | _SC_READER_WRITER_LOCKS |
| _POSIX_REALTIME_SIGNALS | _SC_REALTIME_SIGNALS |
| _POSIX_REGEXP | _SC_REGEXP |
| _POSIX_SAVED_IDS | _SC_SAVED_IDS |
| _POSIX_SEMAPHORES | _SC_SEMAPHORES |
| _POSIX_SHARED_MEMORY_OBJECTS | _SC_SHARED_MEMORY_OBJECTS |
| _POSIX_SHELL | _SC_SHELL |
| _POSIX_SPAWN | _SC_SPAWN |
| _POSIX_SPIN_LOCKS | _SC_SPIN_LOCKS |
| _POSIX_SPORADIC_SERVER | _SC_SPORADIC_SERVER |
| _POSIX_SS_REPL_MAX | _SC_SS_REPL_MAX |
| _POSIX_SYNCHRONIZED_IO | _SC_SYNCHRONIZED_IO |
| _POSIX_THREAD_ATTR_STACKADDR | _SC_THREAD_ATTR_STACKADDR |
| _POSIX_THREAD_ATTR_STACKSIZE | _SC_THREAD_ATTR_STACKSIZE |
| _POSIX_THREAD_CPUTIME | _SC_THREAD_CPUTIME |
| _POSIX_THREAD_PRIO_INHERIT | _SC_THREAD_PRIO_INHERIT |
| _POSIX_THREAD_PRIO_PROTECT | _SC_THREAD_PRIO_PROTECT |
| _POSIX_THREAD_PRIORITY_SCHEDULING | _SC_THREAD_PRIORITY_SCHEDULING |
| _POSIX_THREAD_PROCESS_SHARED | _SC_THREAD_PROCESS_SHARED |
| _POSIX_THREAD_ROBUST_PRIO_INHERIT | _SC_THREAD_ROBUST_PRIO_INHERIT |
| _POSIX_THREAD_ROBUST_PRIO_PROTECT | _SC_THREAD_ROBUST_PRIO_PROTECT |
| _POSIX_THREAD_SAFE_FUNCTIONS | _SC_THREAD_SAFE_FUNCTIONS |
| _POSIX_THREAD_SPORADIC_SERVER | _SC_THREAD_SPORADIC_SERVER |
| _POSIX_THREADS | _SC_THREADS |
| _POSIX_TIMEOUTS | _SC_TIMEOUTS |
| _POSIX_TIMERS | _SC_TIMERS |
| _POSIX_TYPED_MEMORY_OBJECTS | _SC_TYPED_MEMORY_OBJECTS |
| _POSIX_VERSION | _SC_VERSION |
| _POSIX_V8_ILP32_OFF32 | _SC_V8_ILP32_OFF32 |
| _POSIX_V8_ILP32_OFFBIG | _SC_V8_ILP32_OFFBIG |
| _POSIX_V8_LP64_OFF64 | _SC_V8_LP64_OFF64 |
| _POSIX_V8_LPBIG_OFFBIG | _SC_V8_LPBIG_OFFBIG |
| _POSIX_V7_ILP32_OFF32 | _SC_V7_ILP32_OFF32 |
| _POSIX_V7_ILP32_OFFBIG | _SC_V7_ILP32_OFFBIG |
| _POSIX_V7_LP64_OFF64 | _SC_V7_LP64_OFF64 |
| _POSIX_V7_LPBIG_OFFBIG | _SC_V7_LPBIG_OFFBIG |
| _POSIX2_C_BIND | _SC_2_C_BIND |
| _POSIX2_C_DEV | _SC_2_C_DEV |
| _POSIX2_CHAR_TERM | _SC_2_CHAR_TERM |
| _POSIX2_FORT_RUN | _SC_2_FORT_RUN |
| _POSIX2_LOCALEDEF | _SC_2_LOCALEDEF |
| _POSIX2_SW_DEV | _SC_2_SW_DEV |
| _POSIX2_UPE | _SC_2_UPE |
| _POSIX2_VERSION | _SC_2_VERSION |
| _XOPEN_CRYPT | _SC_XOPEN_CRYPT |
| _XOPEN_ENH_I18N | _SC_XOPEN_ENH_I18N |
| _XOPEN_REALTIME | _SC_XOPEN_REALTIME |
| _XOPEN_REALTIME_THREADS | _SC_XOPEN_REALTIME_THREADS |
| _XOPEN_SHM | _SC_XOPEN_SHM |
| _XOPEN_UNIX | _SC_XOPEN_UNIX |
| _XOPEN_UUCP | _SC_XOPEN_UUCP |
| _XOPEN_VERSION | _SC_XOPEN_VERSION |

† 执行单元的性质以及执行单元被视为可用、可以变得可用的精确条件，或者它可以并行执行多少个线程，都是由实现定义的。

## 返回值 (RETURN VALUE)

如果 `name` 是无效值，`sysconf()` 应返回 -1 并设置 `errno` 来指示错误。如果与 `name` 对应的变量在 `<limits.h>` 中被描述为最大值或最小值但该变量没有限制，`sysconf()` 应返回 -1 而不改变 `errno` 的值。注意，不确定的限制并不意味着无限限制；参见 `<limits.h>`。

否则，`sysconf()` 应返回系统上该变量的当前值。返回的值不应比应用程序使用实现的 `<limits.h>` 或 `<unistd.h>` 编译时描述给应用程序的相应值更具限制性。除了 `_SC_NPROCESSORS_ONLN` 之外，为 `name` 参数返回的值在调用进程的生命周期内不应改变，但是 `sysconf(_SC_OPEN_MAX)` 在调用 `setrlimit()` 改变 RLIMIT_NOFILE 软限制之前和之后可能返回不同的值。

如果与 `name` 对应的变量依赖于不支持的选项，则结果未指定。

## 错误 (ERRORS)

`sysconf()` 函数在以下情况下应失败：

- **[EINVAL]** - `name` 参数的值无效。

---

## 应用程序用法 (APPLICATION USAGE)

由于 -1 在成功情况下也是允许的返回值，希望检查错误情况的应用程序应将 `errno` 设置为 0，然后调用 `sysconf()`，如果返回 -1，则检查 `errno` 是否为非零值。

应用程序开发人员应在获取和使用相关变量值（如 `_POSIX_SS_REPL_MAX`）之前检查某个选项（如 `_POSIX_SPORADIC_SERVER`）是否受支持。

虽然 `_SC_NPROCESSORS_CONF` 和 `_SC_NPROCESSORS_ONLN` 查询为一类"高负载"应用程序提供了一种估计可创建的线程最优数量以最大化吞吐量的方法，但现实世界环境存在影响可实现的实际效率的复杂情况。例如：

- 系统上可能运行着多个"高负载"应用程序。
- 系统可能在使用电池电源，应用程序应与系统协调以确保长时间运行的任务可以暂停、恢复并在断电情况下也能成功完成。

如果一个可移植的"高负载"应用程序想要避免使用扩展，其开发人员可能希望基于长时间运行任务的逻辑分区来创建线程，或者利用启发式方法（如 CPU 时间与实时时间的比率）。

## 原理 (RATIONALE)

此功能是为了满足应用程序开发人员和处理许多国际系统配置的系统供应商的要求而添加的。它与 `pathconf()` 和 `fpathconf()` 密切相关。

虽然符合规范的应用程序可以通过从不要求超过 POSIX.1-2024 卷中发布的最小值资源来在所有系统上运行，但该应用程序能够使用任何给定系统上可用资源数量的实际值是有用的。为此，应用程序使用 `<limits.h>` 或 `<unistd.h>` 中符号常量的值。

然而，一旦编译完成，应用程序必须仍然能够处理可用资源量增加的情况。为此，应用程序可能需要一种在执行时确定资源数量或选项存在性的方法。

提供两个示例：

1. 应用程序可能希望在有作业控制和无作业控制的系统上表现不同。希望向计算机架构的所有实例只分发单一二进制包的应用程序供应商，如果只依赖 POSIX.1-2024 卷中发布的 `<unistd.h>` 值，将被迫假设作业控制永远不可用。
2. 国际应用程序供应商有时需要知道每秒时钟滴答数的知识。没有这些功能，他们将需要要么以源代码形式部分分发其应用程序，要么为其运营的各个国家提供 50 Hz 和 60 Hz 版本。

正是许多应用程序实际上以可执行形式广泛分发的认识导致了这个功能的产生。如果仅限于头文件中最具限制性的值，此类应用程序将必须准备接受最小微计算机提供的环境限制。虽然这完全是可移植的，但人们普遍认为它们应该能够利用大型系统提供的功能，而不受源代码和目标代码分发相关的限制。

在讨论此功能期间，有人指出应用程序几乎总是可以通过适当测试各种函数本身来在运行时识别某个值可能是什么。而且，无论如何，它总是可以被编写为充分处理各种函数的错误返回。最终，人们认为这给应用程序开发人员施加了不合理程度的复杂性和成熟度要求。

这个运行时功能并不意味着提供应用程序必须多次检查的不断变化的值。这些值被认为变化的频率不超过每个系统初始化一次，例如由系统管理员或操作员使用自动配置程序进行的初始化。POSIX.1-2024 卷规定它们在进程生命周期内不应改变。

一些值适用于整个系统，而另一些值在文件系统或目录级别变化。后者在 `fpathconf()` 中描述。

注意，所有返回的值必须能够表示为整数。字符串值曾被考虑，但由于其实现和使用的额外复杂性而被拒绝。

某些值（如 {PATH_MAX}）有时太大，不能用于分配数组等操作。`sysconf()` 函数返回负值以表明在这种情况下甚至没有定义这个符号常量。

与 `pathconf()` 类似，这允许实现没有限制。当某个资源无限时，返回表示已达到其他某个资源限制的错误是符合要求的行为。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

- `confstr()`
- `fpathconf()`
- `<limits.h>`
- `<unistd.h>`
- `getconf`

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 The IEEE 的商标。*
*版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利*