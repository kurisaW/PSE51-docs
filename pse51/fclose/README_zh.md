# fclose

## 名称

fclose — 关闭流

## 概要

```c
#include <stdio.h>

int fclose(FILE *stream);
```

## 描述

`fclose()` 函数应该使 `stream` 指向的流被刷新，并且相关联的文件被关闭。流的任何未写入的缓冲数据应该写入文件；任何未读取的缓冲数据应该被丢弃。无论调用是否成功，流都应该与文件解除关联，任何由 `setbuf()` 或 `setvbuf()` 函数设置的缓冲区都应该与流解除关联。如果相关联的缓冲区是自动分配的，应该被释放。

如果文件尚未到达 EOF，并且文件是支持定位的，那么如果流是底层文件描述符的活动句柄，底层打开文件描述符的文件偏移量应该设置为流的文件位置。

如果流是可写的，并且有缓冲数据尚未写入文件，`fclose()` 函数应该标记更新底层文件的最后数据修改时间和最后文件状态改变时间戳。`fclose()` 函数应该执行等效于在与 `stream` 指向的流相关联的文件描述符上执行 `close()` 操作。

在调用 `fclose()` 之后，任何对 `stream` 的使用都会导致未定义行为。

## 返回值

成功完成后，`fclose()` 应该返回 0；否则，应该返回 EOF 并设置 `errno` 来指示错误。

## 错误

`fclose()` 函数在以下情况下可能��败：

- **[EAGAIN]**
  - 为 `stream` 底层的文件描述符设置了 O_NONBLOCK 标志，并且线程将在写操作中被延迟。

- **[EBADF]**
  - 流底层的文件描述符无效。

- **[EFBIG]**
  - 尝试写入一个超过最大文件大小的文件。

- **[EFBIG]**
  - 尝试写入一个超过进程文件大小限制的文件。同时应该为线程生成 SIGXFSZ 信号。

- **[EFBIG]**
  - 文件是常规文件，并且尝试在或超过与相应流相关联的偏移量最大值的位置写入。

- **[EINTR]**
  - `fclose()` 函数被信号中断。

- **[EIO]**
  - 进程是后台进程组的成员，尝试向其控制终端写入，TOSTOP 被设置，调用线程未阻塞 SIGTTOU，进程未忽略 SIGTTOU，且进程的进程组是孤立的。此错误也可能在实现定义的条件下返回。

- **[ENOMEM]**
  - 底层流由 `open_memstream()` 或 `open_wmemstream()` 创建，且可用内存不足。

- **[ENOSPC]**
  - 包含文件的设备上没有剩余可用空间，或者 `fmemopen()` 函数使用的缓冲区中没有剩余空间。

- **[EPIPE]**
  - 尝试写入管道或 FIFO，但没有进程为其打开读取。同时应该向线程发送 SIGPIPE 信号。

`fclose()` 函数在以下情况下可能失败：

- **[ENXIO]**
  - 对不存在的设备发出请求，或者请求超出了设备的能力范围。

## 示例

无。

## 应用程序使用

由于在调用 `fclose()` 后，任何对 `stream` 的使用都会导致未定义行为，因此不应该在 `stdin`、`stdout` 或 `stderr` 上使用 `fclose()`，除非在进程终止前立即使用，以避免在其他依赖这些流的标准接口中触发未定义行为。如果应用程序注册了任何 `atexit()` 处理程序，这样的 `fclose()` 调用应该等到最后一个处理程序完成时才发生。一旦使用 `fclose()` 关闭了 `stdin`、`stdout` 或 `stderr`，就没有标准的方法重新打开这些流中的任何一个。

使用 `freopen()` 来改变 `stdin`、`stdout` 或 `stderr` 而不是关闭它们，可以避免文件在应用程序后期意外地作为特殊文件描述符 STDIN_FILENO、STDOUT_FILENO 或 STDERR_FILENO 之一被打开的危险。

## 原理

无。

## 未来方向

无。

## 参见

- 2.5 标准 I/O 流
- `atexit()`
- `close()`
- `fmemopen()`
- `fopen()`
- `freopen()`
- `getrlimit()`
- `open_memstream()`
- `<stdio.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

添加了大文件峰会扩展。

### Issue 6

超出了 ISO C 标准的扩展被标记。

以下对 POSIX 实现的新要求来自于与单一 UNIX 规范的对齐：

*   添加了 [EFBIG] 错误作为大文件支持扩展的一部分。
*   添加了 [ENXIO] 可选错误条件。

描述部分被更新以说明无论调用是否成功，流和任何缓冲区都会被解除关联。这是为了与 ISO/IEC 9899:1999 标准对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/28，将错误部分中的 [EAGAIN] 错误从"进程将被延迟"更新为"线程将被延迟"。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #002，阐明了文件描述符和流的交互。

从 The Open Group Technical Standard, 2006, Extended API Set Part 1 更新了 [ENOSPC] 错误条件并添加了 [ENOMEM] 错误。

进行了与支持细粒度时间戳相关的更改。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0113 [87], XSH/TC1-2008/0114 [79], 和 XSH/TC1-2008/0115 [14]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0104 [555]。

### Issue 8

应用了 Austin Group Defect 308，阐明了 [EFBIG] 错误的处理。

应用了 Austin Group Defect 1669，从与进程文件大小限制相关的 [EFBIG] 错误部分移除了 XSI 着色。