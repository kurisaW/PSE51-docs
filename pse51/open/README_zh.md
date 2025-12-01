# open, openat — 打开文件

## 概要

```c
#include <sys/stat.h>
#include <fcntl.h>

int open(const char *path, int oflag, ...);
int openat(int fd, const char *path, int oflag, ...);
```

## 描述

`open()` 函数用于建立文件与文件描述符之间的连接。它创建一个引用文件的打开文件描述，以及一个引用该打开文件描述的文件描述符。文件描述符被其他 I/O 函数用来引用该文件。`path` 参数指向为文件命名的路径名。

`open()` 函数应返回指定文件的文件描述符，按照 [2.6 File Descriptor Allocation](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_06) 中描述的方式分配。打开文件描述是新的，因此文件描述符不应与系统中任何其他进程共享它。与新文件描述符关联的 `FD_CLOEXEC` 文件描述符标志应被清除，除非在 `oflag` 中设置了 `O_CLOEXEC` 标志。与新文件描述符关联的 `FD_CLOFORK` 文件描述符标志应被清除，除非在 `oflag` 中设置了 `O_CLOFORK` 标志。

用于标记文件内当前位置的文件偏移量应设置为文件开头。

打开文件描述的文件状态标志和文件访问模式应根据 `oflag` 的值进行设置。

`oflag` 的值通过以下列表中标志的按位包含或来构造，这些标志在 `<fcntl.h>` 中定义。应用程序应在 `oflag` 的值中精确指定以下前五个值（文件访问模式）中的一个：

### 文件访问模式

- **O_EXEC**: 仅用于执行（非目录文件）。如果 `path` 命名一个目录且 `O_EXEC` 与 `O_SEARCH` 不是同一个值，`open()` 应失败。

- **O_RDONLY**: 仅用于读取。

- **O_RDWR**: 用于读取和写入。如果 `path` 命名一个 FIFO，且实现不支持同时为读取和写入打开 FIFO，则 `open()` 应失败。

- **O_SEARCH**: 仅用于搜索目录。如果 `path` 命名一个非目录文件且 `O_SEARCH` 与 `O_EXEC` 不是同一个值，`open()` 应失败。

- **O_WRONLY**: 仅用于写入。

### 附加标志

可以使用以下标志的任何组合：

- **O_APPEND**: 如果设置，每次写入前文件偏移量应设置为文件末尾。

- **O_CLOEXEC**: 如果设置，新文件描述符的 `FD_CLOEXEC` 标志应被设置。

- **O_CLOFORK**: 如果设置，新文件描述符的 `FD_CLOFORK` 标志应被设置。

- **O_CREAT**: 如果文件存在，此标志无效，除非下面 `O_EXCL` 中所述。否则，如果未设置 `O_DIRECTORY`，文件应作为常规文件创建；文件的用户 ID 应设置为进程的有效用户 ID；文件的组 ID 应设置为文件父目录的组 ID 或进程的有效组 ID；文件模式的访问权限位（参见 `<sys/stat.h>`）应设置为 `oflag` 参数后的参数值，类型为 `mode_t`，修改如下：对文件模式位和进程文件模式创建掩码的补码中的相应位执行按位与操作。

- **O_DIRECTORY**: 如果 `path` 解析为非目录文件，失败并将 `errno` 设置为 `[ENOTDIR]`。

- **O_DSYNC** [SIO]: 文件描述符上的写 I/O 操作应按同步 I/O 数据完整性完成的要求完成。

- **O_EXCL**: 如果设置了 `O_CREAT` 和 `O_EXCL`，且文件存在，`open()` 应失败。对文件存在性的检查以及文件不存在时创建文件的操作，相对于其他在相同目录中执行 `open()` 并命名相同文件名且设置了 `O_EXCL` 和 `O_CREAT` 的线程应是原子的。如果设置了 `O_EXCL` 和 `O_CREAT`，且 `path` 命名一个符号链接，`open()` 应失败并将 `errno` 设置为 `[EEXIST]`，无论符号链接的内容如何。

- **O_NOCTTY**: 如果设置且 `path` 标识终端设备，`open()` 不应导致终端设备成为进程的控制终端。

- **O_NOFOLLOW**: 如果 `path` 命名符号链接，失败并将 `errno` 设置为 `[ELOOP]`。

- **O_NONBLOCK**: 当使用 `O_RDONLY` 或 `O_WRONLY` 打开 FIFO 时：
  - 如果设置了 `O_NONBLOCK`，只读 `open()` 应立即返回。只写 `open()` 如果当前没有进程为读取而打开文件，应返回错误。
  - 如果清除了 `O_NONBLOCK`，只读 `open()` 应阻塞调用线程，直到某个线程为写入而打开文件。只写 `open()` 应阻塞调用线程，直到某个线程为读取而打开文件。

  当打开支持非阻塞打开的块特殊文件或字符特殊文件时：
  - 如果设置了 `O_NONBLOCK`，`open()` 函数应立即返回，不等待设备就绪或可用。
  - 如果清除了 `O_NONBLOCK`，`open()` 函数应阻塞调用线程，直到设备就绪或可用后再返回。

- **O_RSYNC** [SIO]: 文件描述符上的读 I/O 操作应完成在与 `O_DSYNC` 和 `O_SYNC` 标志指定的相同完整性级别。

- **O_SYNC** [XSI|SIO]: 文件描述符上的写 I/O 操作应按同步 I/O 文件完整性完成的要求完成。即使不支持同步输入和输出选项，也应支持常规文件的 `O_SYNC` 标志。

- **O_TRUNC**: 如果文件存在且是常规文件，且文件成功以 `O_RDWR` 或 `O_WRONLY` 方式打开，其长度应截断为 0，模式和所有者应保持不变。

- **O_TTY_INIT**: 如果 `path` 标识非伪终端的终端设备，设备尚未在任何进程中打开，且在 `oflag` 中设置了 `O_TTY_INIT` 或 `O_TTY_INIT` 值为零，`open()` 应将任何非标准 `termios` 结构终端参数设置为提供符合性行为的状态，并将与终端关联的 `winsize` 结构初始化为适当的默认设置。

`openat()` 函数应等效于 `open()` 函数，但 `path` 指定相对路径的情况除外。在这种情况下，要打开的文件是相对于与文件描述符 `fd` 关联的目录确定的，而不是相对于当前工作目录确定。

如果 `openat()` 在 `fd` 参数中传递特殊值 `AT_FDCWD`，应使用当前工作目录，行为应与调用 `open()` 完全相同。

## 返回值

成功完成后，这些函数应打开文件并返回一个代表文件描述符的非负整数。否则，这些函数应返回 -1 并设置 `errno` 以指示错误。如果返回 -1，不应创建或修改任何文件。

## 错误

在以下情况下，这些函数应失败：

- **[EACCES]**: 搜索路径前缀某个组件的权限被拒绝，或文件存在但 `oflag` 指定的权限被拒绝，或文件不存在且要创建文件的父目录的写权限被拒绝，或指定了 `O_TRUNC` 但写权限被拒绝。

- **[EEXIST]**: 设置了 `O_CREAT` 和 `O_EXCL`，且指定文件存在。

- **[EILSEQ]**: 指定了 `O_CREAT`，文件不存在，且 `path` 的最后一个路径组件不是可移植文件名且无法在目标目录中创建。

- **[EINTR]**: 在 `open()` 期间捕获到信号。

- **[EINVAL]**: `path` 参数命名一个 FIFO，指定了 `O_RDWR`，且实现认为这是一个错误；或指定了同步 I/O 标志但实现不支持文件的同步 I/O。

- **[EISDIR]**: 指定文件是目录且 `oflag` 包含 `O_WRONLY` 或 `O_RDWR`，或包含不带 `O_DIRECTORY` 的 `O_CREAT`，或在 `O_EXEC` 与 `O_SEARCH` 不是同一个值时包含 `O_EXEC`。

- **[ELOOP]**: 在解析 `path` 参数过程中遇到符号链接循环，或指定了 `O_NOFOLLOW` 且 `path` 参数命名符号链接。

- **[EMFILE]**: 进程可用的所有文件描述符当前都已打开。

- **[ENAMETOOLONG]**: 路径名某个组件的长度超过 `{NAME_MAX}`。

- **[ENFILE]**: 系统当前已打开的文件数量达到最大允许数量。

- **[ENOENT]**: 未设置 `O_CREAT` 且 `path` 的某个组件不命名现有文件，或设置了 `O_CREAT` 且 `path` 路径前缀的某个组件不命名现有文件，或 `path` 指向空字符串。

- **[ENOENT]** 或 **[ENOTDIR]**: 设置了 `O_CREAT`，且 `path` 参数包含至少一个非 `<slash>` 字符并以一个或多个尾部 `<slash>` 字符结尾。

- **[ENOSPC]**: 将包含新文件的目录或文件系统无法扩展，文件不存在，且指定了 `O_CREAT`。

- **[ENOTDIR]**: 路径前缀的某个组件命名一个现有文件，该文件既不是目录也不是指向目录的符号链接；或未指定 `O_CREAT` 和 `O_EXCL`，`path` 参数包含至少一个非 `<slash>` 字符并以一个或多个尾部 `<slash>` 字���结尾；或指定了 `O_DIRECTORY` 且 `path` 参数命名非目录文件。

- **[ENXIO]**: 设置了 `O_NONBLOCK`，指定文件是 FIFO，设置了 `O_WRONLY`，且没有进程为读取而打开文件。

- **[ENXIO]**: 指定文件是字符特殊文件或块特殊文件，与此特殊文件关联的设备不存在。

- **[EOVERFLOW]**: 指定文件是常规文件且文件大小无法在 `off_t` 类型的对象中正确表示。

- **[EROFS]**: 指定文件驻留在只读文件系统上，且在 `oflag` 参数中设置了 `O_WRONLY`、`O_RDWR`、`O_CREAT`（如果文件不存在）或 `O_TRUNC`。

在以下情况下，`openat()` 函数应失败：

- **[EACCES]**: 与 `fd` 关联的打开文件描述的访问模式不是 `O_SEARCH`，且 `fd` 底层目录的权限不允许目录搜索。

- **[EBADF]**: `path` 参数未指定绝对路径且 `fd` 参数既不是 `AT_FDCWD` 也不是为读取或搜索打开的有效文件描述符。

- **[ENOTDIR]**: `path` 参数不是绝对路径且 `fd` 是与非目录文件关联的文件描述符。

在以下情况下，这些函数可能失败：

- **[EAGAIN]** [XSI]: `path` 参数命名被锁定的伪终端设备的从属端。

- **[EINVAL]**: `oflag` 参数的值无效。

- **[ELOOP]**: 在解析 `path` 参数过程中遇到超过 `{SYMLOOP_MAX}` 个符号链接。

- **[ENAMETOOLONG]**: 路径名长度超过 `{PATH_MAX}`，或符号链接的路径名解析产生长度超过 `{PATH_MAX}` 的中间结果。

- **[EOPNOTSUPP]**: `path` 参数命名套接字。

- **[ETXTBSY]**: 文件是正在执行的纯过程（共享文本）文件，且 `oflag` 是 `O_WRONLY` 或 `O_RDWR`。

## 示例

### 按所有者权限写入打开文件

```c
#include <fcntl.h>
...
int fd;
mode_t mode = S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH;
char *pathname = "/tmp/file";
...
fd = open(pathname, O_WRONLY | O_CREAT | O_TRUNC, mode);
...
```

### 使用存在性检查打开文件

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>

#define LOCKFILE "/etc/ptmp"
...
int pfd; /* open() 调用返回的文件描述符整数。 */
...
if ((pfd = open(LOCKFILE, O_WRONLY | O_CREAT | O_EXCL,
    S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH)) == -1)
{
    fprintf(stderr, "Cannot open /etc/ptmp. Try again later.\n");
    exit(1);
}
...
```

### 为写入打开文件

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>

#define LOCKFILE "/etc/ptmp"
...
int pfd;
char pathname[PATH_MAX+1];
...
if ((pfd = open(pathname, O_WRONLY | O_CREAT | O_TRUNC,
    S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH)) == -1)
{
    perror("Cannot open output file\n");
    exit(1);
}
...
```

## 应用程序用法

POSIX.1-2024 不要求终端参数在首次打开时自动设置为任何状态，也不要求在最后一次关闭后重置。为确保设备设置为符合性的初始状态，首次打开终端（伪终端除外）的应用程序应使用 `O_TTY_INIT` 标志进行操作。

除非在 POSIX.1-2024 本卷中另有规定，`oflag` 中允许的标志不是互斥的，可以同时使用任意数量的标志。

`open()` 的 `O_CLOEXEC` 和 `O_CLOFORK` 标志对于避免多线程应用程序中的数据竞争是必要的。

## 另请参见

[`chmod()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/chmod.html)、[`close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html)、[`creat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/creat.html)、[`dirfd()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dirfd.html)、[`dup()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup.html)、[`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)、[`fcntl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fcntl.html)、[`fdopendir()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fdopendir.html)、[`link()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/link.html)、[`lseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/lseek.html)、[`mkdtemp()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkdtemp.html)、[`mknod()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mknod.html)、[`read()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html)、[`symlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/symlink.html)、[`umask()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/umask.html)、[`unlockpt()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/unlockpt.html)、[`write()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/write.html)

XBD [11. General Terminal Interface](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap11.html)、[`<fcntl.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fcntl.h.html)、[`<sys/stat.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_stat.h.html)、[`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html)

---

*The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 IEEE 和 The Open Group*