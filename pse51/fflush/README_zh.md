# fflush — 刷新流

## 概要

```c
#include <stdio.h>

int fflush(FILE *stream);
```

## 描述

本参考页中描述的功能与 ISO C 标准一致。如果此处描述的要求与 ISO C 标准之间存在冲突，则非有意为之。本 POSIX.1-2024 卷遵循 ISO C 标准。

如果 `stream` 指向输出流或更新流且最近操作不是输入，`fflush()` 应使该流的任何未写入数据被写入文件，底层文件的最后数据修改和最后文件状态更改时间戳应被标记为更新。

对于打开用于读取且具有底层文件描述的流，如果文件尚未到达 EOF，并且该文件是可寻址的，底层打开文件描述的文件偏移量应设置为流的文件位置，由 `ungetc()` 或 `ungetwc()` 推回流中且尚未从流中读取的任何字符应被丢弃（不再进一步更改文件偏移量）。

如果 `stream` 是空指针，`fflush()` 应对所有行为如上所述定义的流执行此刷新操作。

## 返回值

成功完成后，`fflush()` 应返回 0；否则，应设置流的错误指示器，返回 EOF，并设置 `errno` 以指示错误。

## 错误

`fflush()` 函数可能失败，如果：

- **[EAGAIN]**
  - 为 `stream` 底层的文件描述符设置了 O_NONBLOCK 标志，且线程将在写操作中延迟。

- **[EBADF]**
  - `stream` 底层的文件描述符无效。

- **[EFBIG]**
  - 尝试写入超过最大文件大小的文件。

- **[EFBIG]**
  - 尝试写入超过进程文件大小限制的文件。还应为该线程生成 SIGXFSZ 信号。

- **[EFBIG]**
  - 文件是常规文件，且尝试写入等于或超过与相应流关联的偏移量最大值。

- **[EINTR]**
  - `fflush()` 函数被信号中断。

- **[EIO]**
  - 进程是后台进程组的成员，尝试写入其控制终端，TOSTOP 已设置，调用线程未阻塞 SIGTTOU，进程未忽略 SIGTTOU，且进程的进程组是孤立的。此错误也可能在实现定义的条件下返回。

- **[ENOMEM]**
  - 底层流由 `open_memstream()` 或 `open_wmemstream()` 创建，且可用内存不足。

- **[ENOSPC]**
  - 包含文件的设备上或 `fmemopen()` 函数使用的缓冲区中没有剩余可用空间。

- **[EPIPE]**
  - 尝试写入没有任何进程打开用于读取的管道或 FIFO。还应向该线程发送 SIGPIPE 信号。

`fflush()` 函数可能失败，如果：

- **[ENXIO]**
  - 向不存在的设备发出请求，或请求超出设备的能力范围。

## 示例

### 向标准输出发送提示

以下示例使用 `printf()` 调用打印一系列提示，用户必须从标准输入输入信息。`fflush()` 调用强制输出到标准输出。使用 `fflush()` 函数是因为标准输出通常是缓冲的，提示可能不会立即打印到输出或终端上。`getline()` 函数调用从标准输入读取字符串并将结果放在变量中，供程序稍后使用。

```c
char *user;
char *oldpasswd;
char *newpasswd;
ssize_t llen;
size_t blen;
struct termios term;
tcflag_t saveflag;

printf("User name: ");
fflush(stdout);
blen = 0;
llen = getline(&user, &blen, stdin);
user[llen-1] = 0;
tcgetattr(fileno(stdin), &term);
saveflag = term.c_lflag;
term.c_lflag &= ~ECHO;
tcsetattr(fileno(stdin), TCSANOW, &term);
printf("Old password: ");
fflush(stdout);
blen = 0;
llen = getline(&oldpasswd, &blen, stdin);
oldpasswd[llen-1] = 0;

printf("\nNew password: ");
fflush(stdout);
blen = 0;
llen = getline(&newpasswd, &blen, stdin);
newpasswd[llen-1] = 0;
term.c_lflag = saveflag;
tcsetattr(fileno(stdin), TCSANOW, &term);
free(user);
free(oldpasswd);
free(newpasswd);
```

## 应用程序使用

无。

## 原理

系统缓冲的数据可能使确定当前文件描述符位置的有效性变得不切实际。因此，POSIX.1-2024 不强制要求在打开用于 `read()` 的流上 `fflush()` 后重新定位文件描述符。

## 未来方向

无。

## 另请参阅

- [2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`fmemopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fmemopen.html)
- [`getrlimit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getrlimit.html)
- [`open_memstream()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/open_memstream.html)
- [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 更改历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

添加了大文件峰会扩展。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源自与单一 UNIX 规范的对齐：

- 添加了 [EFBIG] 错误作为大文件支持扩展的一部分。
- 添加了 [ENXIO] 可选错误条件。

更新了 RETURN VALUE 部分，以注意应为流设置错误指示器。这是为了与 ISO/IEC 9899:1999 标准对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/31，将 ERRORS 部分中的 [EAGAIN] 错误从"进程将被延迟"更新为"线程将被延迟"。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #002，阐明了文件描述符和流的交互。

从 The Open Group Technical Standard, 2006, Extended API Set Part 1 更新了 [ENOSPC] 错误条件并添加了 [ENOMEM] 错误。

修订了 EXAMPLES 部分。

进行了与支持细粒度时间戳相关的更改。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0126 [87], XSH/TC1-2008/0127 [79], 和 XSH/TC1-2008/0128 [14]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0112 [816] 和 XSH/TC2-2008/0113 [626]。

### Issue 8

应用了 Austin Group Defect 308，阐明了 [EFBIG] 错误的处理。

应用了 Austin Group Defect 1669，从 [EFBIG] 错误中删除了与进程文件大小限制相关的 XSI 着色。
