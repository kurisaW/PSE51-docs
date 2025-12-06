# shm_open — 打开共享内存对象 (REALTIME)

## 概要

```c
[SHM] #include <sys/mman.h>

int shm_open(const char *name, int oflag, mode_t mode);
```

## 描述

`shm_open()` 函数应建立共享内存对象与文件描述符之间的连接。它应创建一个引用共享内存对象的打开文件描述，以及一个引用该打开文件描述的文件描述符。文件描述符应按照 [2.6 文件描述符分配](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_06) 中的描述进行分配，并可被其他函数用来引用该共享内存对象。

`name` 参数指向一个命名共享内存对象的字符串。名称是否出现在文件系统中并对以路径名作为参数的其他函数可见是未指定的。`name` 参数符合路径名的构造规则，但 `name` 中除前导 `<slash>` 字符外的 `<slash>` 字符的解释是实现定义的，且 `name` 参数的长度限制是实现定义的，不需要与路径名限制 `{PATH_MAX}` 和 `{NAME_MAX}` 相同。

如果 `name` 以 `<slash>` 字符开头，那么调用 `shm_open()` 时使用相同 `name` 值的进程将引用同一个共享内存对象，只要该名称未被删除。如果 `name` 不以 `<slash>` 字符开头，则效果是实现定义的。

如果成功，`shm_open()` 应返回共享内存对象的文件描述符。打开文件描述是新的，因此文件描述符不与任何其他进程共享它。文件偏移量是否设置是未指定的。与新文件描述符关联的 `FD_CLOEXEC` 文件描述符标志应被设置。

打开文件描述的文件状态标志和文件访问模式应根据 `oflag` 的值进行设置。`oflag` 参数是以下标志的按位包含 OR。应用程序应在 `oflag` 的值中恰好指定以下前两个值（访问模式）之一：

### 访问模式

- `O_RDONLY` - 仅以读访问方式打开。
- `O_RDWR` - 以读或写访问方式打开。

### 附加标志

`oflag` 的值中可以指定剩余标志的任意组合：

- `O_CREAT` - 如果共享内存对象存在，此标志无效，除了如下面 `O_EXCL` 中所述的情况外。否则，创建共享内存对象。共享内存对象的用户 ID 应设置为进程的有效用户 ID。共享内存对象的组 ID 应设置为进程的有效组 ID；但是，如果 `name` 参数在文件系统中可见，组 ID 可以设置为包含目录的组 ID。共享内存对象的权限位应设置为 `mode` 参数的值，但进程的文件模式创建掩码中设置的位除外。当设置了除文件权限位外的 `mode` 中的位时，效果是未指定的。`mode` 参数不影响共享内存对象是为读取、为写入还是为两者打开。共享内存对象的大小为零。

- `O_EXCL` - 如果设置了 `O_EXCL` 和 `O_CREAT`，且共享内存对象存在，则 `shm_open()` 失败。对于其他执行 `shm_open()` 并使用相同共享内存对象名称且设置了 `O_EXCL` 和 `O_CREAT` 的进程，检查共享内存对象是否存在以及创建对象（如果不存在）的操作是原子的。如果设置了 `O_EXCL` 但未设置 `O_CREAT`，则结果是未定义的。

- `O_TRUNC` - 如果共享内存对象存在，并且它成功以 `O_RDWR` 方式打开，则对象应被截断为零长度，模式和工作所有者应不被此函数调用更改。将 `O_TRUNC` 与 `O_RDONLY` 一起使用的结果是未定义的。

### 原子操作

以下函数在对共享内存对象操作时，在 POSIX.1-2024 中指定的效果方面应彼此原子：

- [`close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html)
- [`ftruncate()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ftruncate.html)
- [`mmap()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mmap.html)
- `shm_open()`
- [`shm_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/shm_unlink.html)

如果两个线程各自调用这些函数中的一个，每个调用要么看到另一个调用的所有指定效果，要么都不看到。对 [`close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html) 函数的要求也应适用于文件描述符成功关闭的任何情况，无论是什么原因（例如，作为调用 [`close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html)、调用 [`dup2()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup2.html) 或进程终止的结果）。

当创建共享内存对象时，共享内存对象的状态，包括与共享内存对象关联的所有数据，将持续存在，直到共享内存对象被取消链接且所有其他引用都消失。系统重启后名称和共享内存对象状态是否保持有效是未指定的。

## 返回值

成功完成后，`shm_open()` 函数应返回表示文件描述符的非负整数。否则，它应返回 -1 并设置 `errno` 以指示错误。

## 错误

如果出现以下情况，`shm_open()` 函数应失败：

- **[EACCES]** - 共享内存对象存在且 `oflag` 指定的权限被拒绝，或者共享内存对象不存在且创建共享内存对象的权限被拒绝，或者指定了 `O_TRUNC` 且写权限被拒绝。

- **[EEXIST]** - 设置了 `O_CREAT` 和 `O_EXCL` 且命名的共享内存对象已存在。

- **[EINTR]** - `shm_open()` 操作被信号中断。

- **[EINVAL]** - ���定名称不支持 `shm_open()` 操作。

- **[EMFILE]** - 进程可用的所有文件描述符当前都已打开。

- **[ENFILE]** - 系统中当前打开的共享内存对象过多。

- **[ENOENT]** - 未设置 `O_CREAT` 且命名的共享内存对象不存在。

- **[ENOSPC]** - 创建新共享内存对象的空间不足。

如果出现以下情况，`shm_open()` 函数可能失败：

- **[ENAMETOOLONG]** - `name` 参数的长度在不支持 XSI 选项 [XSI] 的系统上超过 `{_POSIX_PATH_MAX}` 或在 XSI 系统上超过 `{_XOPEN_PATH_MAX}`，或者具有在不支持 XSI 选项 [XSI] 的系统上长于 `{_POSIX_NAME_MAX}` 或在 XSI 系统上长于 `{_XOPEN_NAME_MAX}` 的路径名组件。

## 示例

### 创建和映射共享内存对象

以下代码段演示了使用 `shm_open()` 创建共享内存对象，然后使用 `ftruncate()` 设置其大小，最后使用 `mmap()` 将其映射到进程地址空间：

```c
#include <unistd.h>
#include <sys/mman.h>
...

#define MAX_LEN 10000
struct region {        /* 定义共享内存的"结构" */
    int len;
    char buf[MAX_LEN];
};
struct region *rptr;
int fd;

/* 创建共享内存对象并设置其大小 */

fd = shm_open("/myregion", O_CREAT | O_RDWR, S_IRUSR | S_IWUSR);
if (fd == -1)
    /* 处理错误 */;

if (ftruncate(fd, sizeof(struct region)) == -1)
    /* 处理错误 */;

/* 映射共享内存对象 */

rptr = mmap(NULL, sizeof(struct region),
       PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
if (rptr == MAP_FAILED)
    /* 处理错误 */;

/* 现在我们可以使用 rptr 的字段引用映射区域；
   例如，rptr->len */
...
```

## 应用程序用法

无。

## 基本原理

当支持内存映射文件选项时，使用正常的 `open()` 调用来获取要映射文件的描述符，这是与 `mmap()` 的现有实践保持一致。当支持共享内存对象选项时，`shm_open()` 函数应获取要映射的共享内存对象的描述符。

用文件描述符表示几种类型的对象有充分的先例。在 POSIX.1-1990 标准中，文件描述符可以表示文件、管道、FIFO、tty 或目录。许多实现简单地有一个操作向量，该向量按文件描述符类型索引并执行非常不同的操作。请注意，在某些情况下，传递给文件描述符泛型操作的文件描述符由 `open()` 或 `creat()` 返回，在某些情况下由替代函数（如 `pipe()`）返回。`shm_open()` 使用后一种技术。

请注意，此类共享内存对象实际上可以实现为映射文件。在这两种情况下，都可以在打开后使用 `ftruncate()` 设置大小。`shm_open()` 函数本身不创建指定大小的共享对象，因为这将重复设置文件描述符引用对象大小的现有函数。

在内存对象使用现有文件系统实现的实现上，`shm_open()` 函数可以使用调用 `open()` 的宏来实现，而 `shm_unlink()` 函数可以使用调用 `unlink()` 的宏来实现。

对于没有永久文件系统的实现，允许内存对象的名称定义在系统重启后不保留。请注意，这允许具有永久文件系统的系统也将内存对象实现为实现内部的数据结构。

在选择直接使用内存实现内存对象的实现上，`shm_open()` 后跟 `ftruncate()` 和 `close()` 可用于预分配共享内存区域并设置该预分配的大小。这对于没有虚拟内存硬件支持的系统可能是必要的，以确保内存是连续的。

`shm_open()` 的有效打开标志集被限制为 `O_RDONLY`、`O_RDWR`、`O_CREAT` 和 `O_TRUNC`，因为这些可以在大多数内存映射系统上轻松实现。本 POSIX.1-2024 卷对实现因实现定义的原因（包括硬件原因）无法提供请求的文件访问时的结果保持沉默。`O_CLOEXEC` 打开标志未列出，因为所有共享内存对象创建时都已设置 `FD_CLOEXEC` 标志；应用程序稍后可以使用 `fcntl()` 清除该标志，以允许共享内存文件描述符在 [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html) 函数族中保留。

错误条件 `[EACCES]` 和 `[ENOTSUP]` 的提供是为了告知应用程序实现无法完成请求。

`[EACCES]` 指示由于实现定义的原因（可能是硬件相关），实现无法遵守请求的模式，因为它与另一个请求的模式冲突。一个例子可能是应用程序希望打开内存对象两次，使用不同的访问模式映射不同的区域。如果实现无法将单个区域映射到进程空间中的两个位置（如果两个区域需要不同的访问模式，这是必需的），那么实现可以在第二次打开时通知应用程序。

`[ENOTSUP]` 指示由于实现定义的原因（可能是硬件相关），实现完全无法遵守请求的模式。一个例子是实现的硬件不支持只写共享内存区域。

在所有实现上，为了性能（如 RAM 磁盘）或实现定义的原因（仅在特定文件系统上支持共享内存），可能需要将内存对象的位置限制在特定的文件系统。`shm_open()` 函数可用于强制执行这些限制。应用程序有多种方法可用于确定文件的适当名称或适当目录的位置。一种方法是通过环境变量使用 `getenv()`。另一种方法是来自配置文件。

本 POSIX.1-2024 卷指定内存对象在创建时初始内容为零。这与当前文件和新分配内存的行为一致。对于那些使用物理内存的实现，此类实现可能只是简单地使用可用内存并将其未初始化地给予进程。但是，这与未初始化数据区域、堆栈以及文件的标准行为不一致。最后，出于安全原因，将分配的内存设置为零是非常可取的。因此，需要将内存对象初始化为零。

## 未来方向

未来版本可能要求 `shm_open()` 和 `shm_unlink()` 函数具有类似于正常文件系统操作的语义。

## 另请参见

- [2.6 文件描述符分配](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_06)
- [`close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html)
- [`dup()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup.html)
- [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`fcntl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fcntl.html)
- [`mmap()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mmap.html)
- [`shmat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmat.html)
- [`shmctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmctl.html)
- [`shmdt()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/shmdt.html)
- [`shm_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/shm_unlink.html)
- [`umask()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/umask.html#tag_17_645)
- XBD [`<fcntl.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fcntl.h.html)
- XBD [`<sys/mman.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_mman.h.html)

## 变更历史

首次在 Issue 5 中发布。包含用于与 POSIX 实时扩展对齐。

### Issue 6

`shm_open()` 函数被标记为共享内存对象选项的一部分。

`[ENOSYS]` 错误条件已被删除，因为如果实现不支持共享内存对象选项，则不需要提供存根。

IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/126 被应用，在示例部分添加了示例。

### Issue 7

Austin Group 解释 1003.1-2001 #077 被应用，阐明了 `name` 参数并将 `[ENAMETOOLONG]` 从"应失败"错误更改为"可能失败"错误。

Austin Group 解释 1003.1-2001 #141 被应用，添加了未来方向。

SD5-XSH-ERN-170 被应用，更新描述以阐明设置共享内存对象用户 ID 和组 ID 的措辞。

POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0324 [835]、XSH/TC2-2008/0325 [835] 和 XSH/TC2-2008/0326 [835] 被应用。

### Issue 8

Austin Group 缺陷 411 被应用，在基本原理部分添加了关于 `O_CLOEXEC` 的句子。

Austin Group 缺陷 593 被应用，从描述部分删除了对 [`<fcntl.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fcntl.h.html) 的引用。

Austin Group 缺陷 695 被应用，为共享内存对象上的操作添加了原子性要求。

---
