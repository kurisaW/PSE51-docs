# close, posix_close

## SYNOPSIS（概要）

```c
#include <unistd.h>

int close(int fildes);
int posix_close(int fildes, int flag);
```

## DESCRIPTION（描述）

`close()` 函数应释放由 `fildes` 指示的文件描述符。释放意味着使该文件描述符可通过后续的 `open()` 调用或其他分配文件描述符的函数返回。调用进程在该文件描述符关联的文件上拥有的所有进程拥有的文件锁都应被解锁。

如果 `close()` 被一个要捕获的信号中断，那么它是不确定的：是返回 -1 并将 `errno` 设置为 [EINTR] 且 `fildes` 保持打开状态，还是返回 -1 并将 `errno` 设置为 [EINPROGRESS] 且 `fildes` 被关闭，或是返回 0 表示成功完成；除非 POSIX_CLOSE_RESTART 被定义为 0，在这种情况下，返回 -1 并将 `errno` 设置为 [EINTR] 且 `fildes` 保持打开状态的选项不会发生。如果 `close()` 返回 -1 并将 `errno` 设置为 [EINTR]，那么除了 `close()` 或 `posix_close()` 之外，`fildes` 是否可以后续传递给任何函数而不出错是不确定的。对于所有其他错误情况（除了 [EBADF]，其中 `fildes` 是无效的），`fildes` 应被关闭。如果即使关闭操作未完成 `fildes` 也被关闭了，关闭操作应继续异步进行，进程应没有进一步跟踪关闭操作完成或最终状态的能力。

当与管道或 FIFO 特殊文件关联的所有文件描述符都被关闭时，管道或 FIFO 中剩余的任何数据都应被丢弃。

当与打开文件描述关联的所有文件描述符都被关闭时，打开文件描述应被释放。

如果文件的链接计数为 0，当与文件关联的所有文件描述符都被关闭时，文件占用的空间应被释放，文件应不再可访问。

[XSI] 如果 `fildes` 指的是伪终端的管理方端，并且这是最后一次关闭，应向控制进程发送 SIGHUP 信号（如果有的话），对于该进程，伪终端的附属方端是控制终端。关闭伪终端的管理方端是否会刷新所有排队的输入和输出是不确定的。

当调用 `close()` 时，如果存在针对 `fildes` 的未完成的可取消异步 I/O 操作，该 I/O 操作可能会被取消。未被取消的 I/O 操作会像 `close()` 操作尚未发生一样完成。所有未被取消的操作应像 `close()` 阻塞直到操作完成为止一样完成。`close()` 操作本身不需要阻塞等待此类 I/O 完成。任何 I/O 操作是否被取消，以及 `close()` 时哪些 I/O 操作可能被取消，是由实现定义的。

如果在最后一次关闭时内存映射文件 [SHM] 或共享内存对象仍然被引用（即进程已映射它），那么内存对象的全部内容应持续存在，直到内存对象变为未被引用。如果这是内存映射文件 [SHM] 或共享内存对象的最后一次关闭，并且关闭导致内存对象变为未被引用，且内存对象已被取消链接，那么内存对象应被移除。

当与套接字关联的所有文件描述符都被关闭时，套接字应被销毁。如果套接字处于连接模式，并且为套接字设置了 SO_LINGER 选项且具有非零延迟时间，并且套接字有未传输的数据，那么 `close()` 应阻塞最多当前延迟间隔，直到所有数据都被传输。

`posix_close()` 函数应等同于 `close()` 函数，除了基于 `flag` 参数的如下所述的修改。如果 `flag` 为 0，那么 `posix_close()` 不应返回 -1 并将 `errno` 设置为 [EINTR]，这意味着 `fildes` 将始终被关闭（除了 [EBADF]，其中 `fildes` 是无效的）。如果 `flag` 包含 POSIX_CLOSE_RESTART 且 POSIX_CLOSE_RESTART 被定义为非零值，并且 `posix_close()` 被一个要捕获的信号中断，那么 `posix_close()` 可能返回 -1 并将 `errno` 设置为 [EINTR]，在这种情况下 `fildes` 应保持打开状态；但是，除了 `close()` 或 `posix_close()` 之外，`fildes` 是否可以后续传递给任何函数而不出错是不确定的。如果 `flag` 无效，`posix_close()` 可能失败并将 errno 设置为 [EINVAL]，但在其他情况下应像 `flag` 为 0 一样行为并关闭 `fildes`。

## RETURN VALUE（返回值）

成功完成后，应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## ERRORS（错误）

`close()` 和 `posix_close()` 函数在以下情况下应失败：

- **[EBADF]**
  `fildes` 参数不是一个打开的文件描述符。

- **[EINPROGRESS]**
  函数被信号中断，`fildes` 被关闭但关闭操作正在异步继续。

`close()` 和 `posix_close()` 函数在以下情况下可能失败：

- **[EINTR]**
  函数被信号中断，POSIX_CLOSE_RESTART 被定义为非零值，并且（在 `posix_close()` 的情况下）`flag` 参数包含 POSIX_CLOSE_RESTART，在这种情况下 `fildes` 仍然打开。

- **[EIO]**
  在从文件系统读取或向文件系统写入时发生 I/O 错误。

`posix_close()` 函数在以下情况下可能失败：

- **[EINVAL]**
  `flag` 参数的值无效。

`close()` 和 `posix_close()` 函数不应返回 [EAGAIN] 或 [EWOULDBLOCK] 错误。如果 POSIX_CLOSE_RESTART 为零，`close()` 函数不应返回 [EINTR] 错误。`posix_close()` 函数不应返回 [EINTR] 错误，除非 `flag` 包含非零的 POSIX_CLOSE_RESTART。

---

*以下部分为参考信息。*

## EXAMPLES（示例）

### 重新分配文件描述符

以下示例关闭当前进程与标准输出关联的文件描述符，将标准输出重新分配给一个新的文件描述符，然后关闭原始文件描述符进行清理。此示例假设文件描述符 0（标准输入的描述符）没有被关闭。

```c
#include <unistd.h>
...
int pfd;
...
close(1);
dup(pfd);
close(pfd);
...
```

顺便说一下，这完全可以使用以下方式实现：

```c
dup2(pfd, 1);
close(pfd);
```

### 关闭文件描述符

在以下示例中，`close()` 用于在尝试将文件描述符与流关联失败后关闭该文件描述符。

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

#define LOCKFILE "/etc/ptmp"
...
int pfd;
FILE *fpfd;
...
if ((fpfd = fdopen (pfd, "w")) == NULL) {
    close(pfd);
    unlink(LOCKFILE);
    exit(1);
}
...
```

## APPLICATION USAGE（应用程序使用）

使用了 `stdio` 例程 `fopen()` 打开文件的应用程序应使用相应的 `fclose()` 例程而不是 `close()`。一旦文件被关闭，文件描述符就不再存在，因为与其对应的整数不再引用文件。

实现可能使用必须继承到子进程的文件描述符，以使子进程保持符合规范，例如用于消息目录或跟踪目的。因此，对任意整数调用 `close()` 的应用程序有不符合规范行为的风险，`close()` 只能可移植地用于应用程序通过显式操作获得的文件描述符值，以及与标准文件流对应的三个文件描述符。在多线程父应用程序中，在 `fork()` 之后和 `exec` 调用之前在循环中调用 `close()` 以避免将意外文件描述符泄漏到子进程的竞争条件的做法因此是不安全的，竞争应通过使用 FD_CLOEXEC 位设置打开所有文件描述符来对抗，除非文件描述符旨在跨 `exec` 继承。

在文件描述符 STDIN_FILENO、STDOUT_FILENO 或 STDERR_FILENO 上使用 `close()` 应立即跟随重新打开这些文件描述符的操作。如果这些文件描述符中的任何一个保持关闭状态（例如，来自 `perror()` 的 [EBADF] 错误），或者如果应用程序后面不相关的 `open()` 或类似调用意外地将文件分配给这些众所周知的文件描述符之一，将导致意外行为。此外，`close()` 后跟重新打开操作（例如，`open()`、`dup()` 等）不是原子操作；应使用 `dup2()` 来更改标准文件描述符。

## RATIONALE（原理）

应避免使用可中断设备关闭例程，以避免与文件描述符的隐式关闭（如通过 `exec`、进程终止或 `dup2()`）相关的问题。POSIX.1-2024 的这一卷仅通过为具有非零 POSIX_CLOSE_RESTART 的 `close()` 和 `posix_close()` 指定 [EINTR] 错误条件来允许此类行为，以便为应用程序提供可移植的方式，在接收到中断后恢复等待与关闭操作相关的事件（例如，磁带机倒带）。本标准还允许实现如果选择不提供重新启动中断关闭操作的方式，则将 POSIX_CLOSE_RESTART 定义为 0。尽管在 [EINTR] 时文件描述符保持打开状态，但它可能不再可用���也就是说，将其传递给除 `close()` 或 `posix_close()` 之外的任何函数可能导致诸如 [EIO] 之类的错误。如果应用程序必须保证数据不会丢失，建议应用程序在关闭操作之前使用 `fsync()` 或 `fdatasync()`，而不是让关闭操作处理待处理的 I/O 并承担中断的风险。

本标准的早期版本在 [EINTR] 和 [EIO] 等错误之后将 `fildes` 的状态保留为未指定；实现在 `close()` 在 [EINTR] 后是否保持 `fildes` 打开方面存在差异。一旦引入线程，这是不令人满意的，因为多线程应用程序需要知道 `fildes` 是否已被关闭。应用程序不能盲目地再次调用 `close()`，因为如果 `fildes` 被第一次调用关闭，另一个线程可能已被分配了一个与 `fildes` 相同值的文件描述符，该文件描述符绝不能被第一个线程关闭。另一方面，从不重试 `close()` 的替代方案将在 `close()` 未关闭 `fildes` 的实现中导致文件描述符泄漏，尽管如果进程即将退出或文件描述符被标记为 FD_CLOEXEC 且进程即将被 `exec` 替换，这种泄漏可能是无害的。本标准引入了带有 `flag` 参数的 `posix_close()`，它允许在两种可能的错误行为之间进行选择，并将 `close()` 实现两种行为中的哪一种保留为未指定（尽管保证是 `posix_close()` 的两种行为之一，而不是像标准早期版本那样完全未指定）。

请注意，标准要求 `close()` 和 `posix_close()` 必须在 [EINTR] 后保持 `fildes` 打开（在允许 [EINTR] 的情况下），并且必须关闭文件描述符，无论所有其他错误如何（除了 [EBADF]，其中 `fildes` 已经无效）。通常，可移植的应用程序只应在检查 [EINTR] 后重试 `close()`（并且在 POSIX_CLOSE_RESTART 被定义为零的实现上，此重试循环将是死代码），如果不尝试重试循环则有文件描述符泄漏的风险。还应注意，[EINTR] 只有在 `close()` 可以被中断时才可能；如果没有安装信号处理程序，那么 `close()` 不会被中断。相反，如果单线程应用程序可以保证在信号处理程序中没有文件描述符被打开或关闭，那么不检查 [EINTR] 的重试循环将是无害的（因为重试将因 [EBADF] 而失败），但保证没有外部库引入线程的使用可能很困难。对于只会在 POSIX_CLOSE_RESTART 被定义为 0 的系统上使用的应用程序，还有额外的保证。这些观察应有助于确定应用程序是否需要将其 `close()` 调用审核为替换为 `posix_close()`。

还应注意，`posix_close()` 在 `flag` 为 0 时总是关闭 `fildes` 的要求，即使在报告错误时也是如此，这与 `fclose()` 总是释放流的要求相似，即使在刷新数据时遇到错误也是如此。

以前总是关闭 `fildes` 的实现可以通过在 `close()` 中将 [EINTR] 转换为 [EINPROGRESS] 来满足新要求；并且可以将 POSIX_CLOSE_RESTART 定义为 0 而不必添加重新启动语义。另一方面，以前在 [EINTR] 时保持 `fildes` 打开的实现可以将其映射到带有 POSIX_CLOSE_RESTART 的 `posix_close()`，并且必须在 `flag` 为 0 时添加 `posix_close()` 的语义；一种可能性是调用原始的 `close()` 实现，检查失败，并在 [EINTR] 时，使用类似于 `dup2()` 的操作用另一个文件描述符替换不完整的关闭操作，该文件描述符可以通过对原始 `close()` 的另一次调用立即关闭，所有这些都在返回应用程序之前。无论哪种方式，`close()` 应始终映射到 `posix_close()` 的两种行为之一，并鼓励实现保持 `close()` 的行为不变，以免破坏依赖本标准早期版本未指定的行为的旧应用程序的实现特定期望。

标准开发人员考虑引入一个线程局部变量，`close()` 将设置该变量以指示它在返回 -1 时是否关闭了 `fildes`。然而，这被拒绝，转而采用更简单的解决方案，即收紧 `close()` 以保证除了 [EINTR] 之外 `fildes` 被关闭，并通过添加 `posix_close()` 来暴露是否期望 [EINTR] 的选择。此外，虽然名称 `posix_close()` 是本标准的新名称，但它让人联想到至少一个实现引入了名为 `close_nocancel()` 的替代系统调用，以允许应用程序选择是否需要重新启动语义。

另一个考虑是实现是否可能返回 [EAGAIN] 作为扩展，以及 `close()` 是否应被要求在这种情况下保持文件描述符打开，因为 [EAGAIN] 通常意味着操作应被重试。任何实现似乎都不太可能有正当理由返回 [EAGAIN] 或 [EWOULDBLOCK]，因此这个要求将意味着应用程序必须包含永远不会使用的错误情况的代码。因此 `close()` 现在被禁止返回 [EAGAIN] 和 [EWOULDBLOCK] 错误。

请注意，对套接字上的 `close()` 阻塞最多当前延迟间隔的要求不是以 O_NONBLOCK 设置为条件的。

标准开发人员拒绝了将 `closefrom()` 添加到标准的提议。因为标准允许实现使用继承的文件描述符作为为子进程提供符合规范环境的手段，所以不可能标准化关闭高于某个值的任意文件描述符同时仍然保证符合规范环境的接口。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（另见）

- `dup()`
- `exec`
- `exit()`
- `fclose()`
- `fopen()`
- `fork()`
- `open()`
- `perror()`
- `unlink()`
- XBD `<unistd.h>`

## CHANGE HISTORY（变更历史）

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 5

DESCRIPTION 为与 POSIX 实时扩展对齐而更新。

### Issue 6

与基于 STREAMS 的文件或伪终端相关的 DESCRIPTION 被标记为 XSI STREAMS 选项组的一部分。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- [EIO] 错误条件作为可选错误添加。
- DESCRIPTION 更新为描述如果发生 I/O 错误并返回 [EIO] 错误条件，`fildes` 文件描述符的状态为未指定。

关于套接字的文本被添加到 DESCRIPTION。

DESCRIPTION 为与 IEEE Std 1003.1j-2000 对齐而更新，指定共享内存对象和内存映射文件（而不是类型化内存对象）是最后一次关闭段落适用的内存对象类型。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/12，更正与伪终端管理方端相关的 XSH 阴影文本。变更的原因是，在这种情况下，伪终端和常规终端的行为应尽可能相似；变更实现了这一点并符合历史行为。

### Issue 7

与 XSI STREAMS 选项相关的功能被标记为过时。

与异步输入和输出以及内存映射文件选项相关的功能被移至 Base。

应用 Austin Group Interpretation 1003.1-2001 #139，阐明对套接字上的 `close()` 阻塞最多当前延迟间隔的要求不是以 O_NONBLOCK 设置为条件的。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0059 [419], XSH/TC1-2008/0060 [149], 和 XSH/TC1-2008/0061 [149]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0069 [555]。

### Issue 8

应用 Austin Group Defect 529，添加 `posix_close()` 函数并更改 `close()` 函数与 [EINTR] 相关的要求。

应用 Austin Group Defect 768，添加 OFD 拥有的文件锁。

应用 Austin Group Defect 1330，移除过时接口。

应用 Austin Group Defect 1466，更改用于伪终端设备的术语。

应用 Austin Group Defect 1525，阐明套接字直到与其关联的所有文件描述符都被关闭时才被销毁。

---

