# sigaction

## 概要

```c
#include <signal.h>

int sigaction(int sig, const struct sigaction *restrict act,
              struct sigaction *restrict oact);
```

## 描述

`sigaction()` 函数允许调用进程检查和/或指定与特定信号相关联的动作。参数 `sig` 指定信号；可接受的值在 `<signal.h>` 中定义。

用于描述要执行动作的 **sigaction** 结构在 `<signal.h>` 头文件中定义，至少包含以下成员：

| 成员类型 | 成员名称 | 描述 |
|-------------|-------------|-------------|
| `void (*)(int)` | `sa_handler` | 指向信号捕获函数的指针，或宏 SIG_IGN 或 SIG_DFL 之一。 |
| `sigset_t` | `sa_mask` | 在执行信号捕获函数期间要阻塞的附加信号集合。 |
| `int` | `sa_flags` | 影响信号行为的特殊标志。 |
| `void (*)(int, siginfo_t *, void *)` | `sa_sigaction` | 指向信号捕获函数的指针。 |

`sa_handler` 和 `sa_sigaction` 占用的存储空间可能重叠，符合规范的应用程序不应同时使用两者。

如果参数 `act` 不是空指针，它指向一个结构，该结构指定要与指定信号相关联的动作。如果参数 `oact` 不是空指针，则先前与该信号相关联的动作将存储在参数 `oact` 指向的位置。如果参数 `act` 是空指针，则信号处理保持不变；因此，该调用可用于查询给定信号的当前处理方式。SIGKILL 和 SIGSTOP 信号不应使用此机制添加到信号掩码中；此限制应由系统强制执行，而不会指示错误。

如果在 **sigaction** 结构的 `sa_flags` 字段中清除了 SA_SIGINFO 标志（见下文），则 `sa_handler` 字段标识要与指定信号相关联的动作。如果在 `sa_flags` 字段中设置了 SA_SIGINFO 标志，则 `sa_sigaction` 字段指定信号捕获函数。

`sa_flags` 字段可用于修改指定信号的行为。

以下在 `<signal.h>` 头文件中定义的标志可以在 `sa_flags` 中设置：

### SA_NOCLDSTOP
当子进程停止[XSI]或已停止的子进程继续时，不产生 SIGCHLD。

如果 `sig` 是 SIGCHLD 且未在 `sa_flags` 中设置 SA_NOCLDSTOP 标志，并且实现支持 SIGCHLD 信号，那么当其任何子进程停止[XSI]时，应为调用进程生成 SIGCHLD 信号，并且当其任何已停止的子进程继续时，可以为调用进程生成 SIGCHLD 信号。如果 `sig` 是 SIGCHLD 且在 `sa_flags` 中设置了 SA_NOCLDSTOP 标志，则实现不应以这种方式生成 SIGCHLD 信号。

### SA_ONSTACK
[XSI] 如果设置且已使用 `sigaltstack()` 声明了替代信号栈，则信号应在该栈上传递给调用进程。否则，信号应在当前栈上传递。

### SA_RESETHAND
如果设置，信号的处置应在进入信号处理程序时重置为 SIG_DFL，并清除 SA_SIGINFO 标志。

**注意：** SIGILL 和 SIGTRAP 在传递时不能自动重置；系统会静默地强制执行此限制。

否则，信号的处置在进入信号处理程序时不应被修改。

此外，如果设置了此标志，`sigaction()` 的行为可能如同也设置了 SA_NODEFER 标志。

### SA_RESTART
此标志影响可中断函数的行为；即那些被指定为因 `errno` 设置为 [EINTR] 而失败的函数。如果设置，且被指定为可中断的函数被此信号中断，则函数应重新启动且不会因 [EINTR] 而失败，除非另有指定。如果使用超时的可中断函数重新启动，则重新启动后的超时持续时间被设置为不超过原始超时值的未指定值。如果未设置标志，被此信号中断的可中断函数应因 `errno` 设置为 [EINTR] 而失败。

### SA_SIGINFO
如果清除且捕获了信号，信号捕获函数应按以下方式进入：

```c
void func(int signo);
```

其中 `signo` 是信号捕获函数的唯一参数。在这种情况下，应用程序应使用 `sa_handler` 成员来描述信号捕获函数，且应用程序不应修改 `sa_sigaction` 成员。

如果设置了 SA_SIGINFO 且捕获了信号，信号捕获函数应按以下方式进入：

```c
void func(int signo, siginfo_t *info, void *context);
```

其中两个附加参数被传递给信号捕获函数。第二个参数应指向 **siginfo_t** 类型的对象，解释生成信号的原因；第三个参数可以转换为指向 **ucontext_t** 类型对象的指针，以引用在信号传递时被中断的接收线程的上下文。在这种情况下，应用程序应使用 `sa_sigaction` 成员来描述信号捕获函数，且应用程序不应修改 `sa_handler` 成员。

`si_signo` 成员包含系统生成的信号编号。

[XSI] `si_errno` 成员可能包含实现定义的附加错误信息；如果非零，它包含一个错误编号，标识导致信号生成的条件。

`si_code` 成员包含标识信号原因的代码，如 *2.4.3 信号动作* 中所述。

### SA_NOCLDWAIT
[XSI] 如果 `sig` 不等于 SIGCHLD，则行为未指定。否则，SA_NOCLDWAIT 标志的行为如 *进程终止的后果* 中所述。

### SA_NODEFER
如果设置且捕获了 `sig`，则 `sig` 不应在线程进入信号处理程序时添加到线程的信号掩码中，除非它包含在 `sa_mask` 中。否则，`sig` 应始终在进入信号处理程序时添加到线程的信号掩码中。

当信号被 `sigaction()` 安装的信号捕获函数捕获时，会计算新的信号掩码并在信号捕获函数期间（或直到调用 `sigprocmask()` 或 `sigsuspend()`）安装。此掩码通过取当前信号掩码与正在传递的信号的 `sa_mask` 值的并集形成，并且除非设置了 SA_NODEFER 或 SA_RESETHAND，还包括正在传递的信号。当用户的信号处理程序正常返回时，原始信号掩码被恢复。

一旦为特定信号安装了动作，它应保持安装状态，直到明确请求另一个动作（通过另一次 `sigaction()` 调用），直到 SA_RESETHAND 标志导致处理程序重置，或者直到调用某个 `exec` 函数。

如果 `sig` 的先前动作是由 `signal()` 建立的，则在 `oact` 指向的结构中返回的字段值是未指定的，特别是 `oact->sa_handler` 不一定是传递给 `signal()` 的相同值。但是，如果指向相同结构或其副本的指针通过 `act` 参数传递给后续的 `sigaction()` 调用，则信号的处理应如同重复对 `signal()` 的原始调用。

如果 `sigaction()` 失败，则不会安装新的信号处理程序。

将无法捕获或忽略的信号的动作设置为 SIG_DFL 的尝试是被忽略还是导致返回错误且 `errno` 设置为 [EINVAL] 是未指定的。

如果未在 `sa_flags` 中设置 SA_SIGINFO，则当 `sig` 已处于挂起状态时后续出现的信号的处置是实现定义的；信号捕获函数应使用单个参数调用。如果在 `sa_flags` 中设置了 SA_SIGINFO，则由 `sigqueue()` 生成的或作为支持指定应用程序定义值的任何信号生成函数结果的后续 `sig` 出现（当 `sig` 已处于挂起状态时）应按 FIFO 顺序排队，直到传递或接受；信号捕获函数应使用三个参数调用。应用程序指定的值作为 **siginfo_t** 结构的 `si_value` 成员传递给信号捕获函数。

在同一进程中同时使用 `sigaction()` 和 `sigwait()` 函数处理相同信号的结果是未指定的。

## 返回值

成功完成后，`sigaction()` 应返回 0；否则应返回 -1，`errno` 应设置为指示错误，且不会安装新的信号捕获函数。

## 错误

`sigaction()` 函数在以下情况下应失败：

- **[EINVAL]** - `sig` 参数不是有效的信号编号，或者尝试捕获无法捕获的信号或忽略无法忽略的信号。

`sigaction()` 函数在以下情况下可能失败：

- **[EINVAL]** - 尝试将无法捕获或忽略（或两者）的信号的动作设置为 SIG_DFL。

此外，在支持 XSI 选项的系统上，如果对于不在 SIGRTMIN 到 SIGRTMAX 范围内的信号，在 **sigaction** 结构的 `sa_flags` 字段中设置了 SA_SIGINFO 标志，`sigaction()` 函数可能失败。

*以下章节为信息性内容。*

## 示例

### 建立信号处理程序

以下示例演示了使用 `sigaction()` 为 SIGINT 信号建立处理程序。

```c
#include <signal.h>

static void handler(int signum)
{
    /* 对信号传递采取适当的行动 */
}

int main(void)
{
    struct sigaction sa;

    sa.sa_handler = handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = SA_RESTART; /* 如果被处理程序中断，重新启动函数 */
    if (sigaction(SIGINT, &sa, NULL) == -1)
        /* 处理错误 */;

    /* 进一步的代码 */
}
```

## 应用程序使用

`sigaction()` 函数取代了 `signal()` 函数，应优先使用。特别是，`sigaction()` 和 `signal()` 不应在同一进程中用于控制相同信号。异步信号安全函数的行为，如在其各自的 DESCRIPTION 部分中所定义的，如 POSIX.1-2024 本卷所指定的，无论是否从信号捕获函数调用。这就是异步信号安全函数可以在信号捕获函数中无限制使用的语句的唯一预期含义。应用程序仍必须考虑此类函数对数据结构、文件和进程状态等所有影响。特别是，应用程序开发人员需要考虑中断 `sleep()` 时的交互限制以及文件描述符的多个句柄之间的交互。任何特定函数被列为异步信号安全的事实并不一定意味着推荐从信号捕获函数调用该函数。

为防止中断非异步信号安全函数调用而产生的错误，应用程序应通过阻塞适当的信号或通过使用某些程序信号量（参见 `semget()`、`sem_init()`、`sem_open()` 等）来保护对这些函数的调用。特别要注意，即使是"安全"函数也可能修改 `errno`；信号捕获函数，如果不作为独立线程执行，应保存和恢复其值，以避免在设置 `errno` 的函数错误返回与随后的 `errno` 检查之间传递信号可能导致信号捕获函数更改 `errno` 值的可能性。自然，相同的原理适用于应用程序例程和异步数据访问的异步信号安全性。注意 `longjmp()` 和 `siglongjmp()` 不在异步信号安全函数列表中。这是因为在 `longjmp()` 和 `siglongjmp()` 之后执行的代码可以调用任何不安全的函数，其危险性与直接从信号处理程序调用这些不安全函数相同。在信号处理程序中使用 `longjmp()` 和 `siglongjmp()` 的应用程序需要严格保护才能具有可移植性。许多被排除在列表之外的其他函数传统上使用 `malloc()` 或 `free()` 函数或标准 I/O 库实现，这两者传统上以非异步信号安全的方式使用数据结构。由于使用公共数据结构的不同函数的任何组合都可能导致异步信号安全问题，POSIX.1-2024 本卷未定义在中断不安全函数的信号处理程序中调用任何不安全函数时的行为。

通常，信号在信号传递之前生效的栈上执行。可以指定替代栈来接收被捕获信号的子集。

当信号处理程序返回时，接收线程在被中断的点恢复执行，除非信号处理程序做出其他安排。如果使用 `longjmp()` 离开信号处理程序，则必须显式恢复信号掩码。

POSIX.1-2024 本卷将设置 SA_SIGINFO 时信号处理函数的第三个参数定义为 `void *` 而不是 `ucontext_t *`，但不要求类型检查。新应用程序应将信号处理函数的第三个参数显式转换为 `ucontext_t *`。

本卷 POSIX.1-2024 不支持 BSD 可选的四参数信号处理函数。BSD 声明应为：

```c
void handler(int sig, int code, struct sigcontext *scp, char *addr);
```

其中 `sig` 是信号编号，`code` 是某些信号的附加信息，`scp` 是指向 **sigcontext** 结构的指针，`addr` 是附加地址信息。在设置 SA_SIGINFO 时指定的信号处理程序的第二个参数指向的对象中提供了大致相同的信息。

由于 `sigaction()` 函数在应用程序设置 SA_RESETHAND 标志时允许但不要求设置 SA_NODEFER，因此依赖新安装信号处理程序的 SA_RESETHAND 功能的应用程序必须在设置 SA_RESETHAND 时始终显式设置 SA_NODEFER 才能具有可移植性。

另请参见 XRAT *B.2.4.2 实时信号生成和传递*中关于实时信号生成和传递的基本原理。

## 基本原理

尽管 POSIX.1-2024 本卷要求不可忽略的信号在进入信号捕获函数时不应添加到信号掩码中，但没有明确要求后续的 `sigaction()` 调用在 `oact` 参数返回的信息中反映这一点。换句话说，如果 SIGKILL 包含在 `act` 的 `sa_mask` 字段中，则后续的 `sigaction()` 调用返回时 `oact` 的 `sa_mask` 字段中是否包含 SIGKILL 是未指定的。

在 `act->sa_flags` 参数中提供 SA_NOCLDSTOP 标志时，允许用 System V 语义重载 SIGCHLD，即每个 SIGCLD 信号指示一个终止的子进程。大多数捕获 SIGCHLD 的符合规范的应用程序预期安装信号捕获函数，该函数重复调用设置了 WNOHANG 标志的 `waitpid()` 函数，对返回状态的每个子进程采取行动，直到 `waitpid()` 返回零。如果对已停止的子进程不感兴趣，使用 SA_NOCLDSTOP 标志可以防止在它们停止时调用信号捕获例程的开销。

一些历史实现还定义了停止进程的其他机制，如 `ptrace()` 函数。这些实现通常不...