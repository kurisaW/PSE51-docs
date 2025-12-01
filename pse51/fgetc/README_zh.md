# fgetc — 从流中获取一个字节

## SYNOPSIS

```c
#include <stdio.h>

int fgetc(FILE *stream);
```

## DESCRIPTION

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 遵从 ISO C 标准。

如果指向 `stream` 的输入流的文件结束指示符未设置且存在下一个字节，`fgetc()` 函数应从指向 `stream` 的输入流中获取下一个字节作为转换为 **int** 的 **unsigned char**，并推进该流的相关文件位置指示符（如果已定义）。由于 `fgetc()` 操作字节，读取由多个字节组成的字符（或"多字节字符"）可能需要多次调用 `fgetc()`。

`fgetc()` 函数可以标记与 `stream` 关联的文件的最后数据访问时间戳以进行更新。最后一次数据访问时间戳应在第一次成功执行 `fgetc()`、`fgets()`、`fread()`、`fscanf()`、`getc()`、`getchar()`、`getdelim()`、`getline()` 或 `scanf()` 时标记为更新，这些函数使用 `stream` 返回的数据不是由先前的 `ungetc()` 调用提供的。

## RETURN VALUE

成功完成时，`fgetc()` 应返回从指向 `stream` 的输入流中获取的下一个字节。如果流的文件结束指示符已设置，或者流处于文件结束状态，流的文件结束指示符应被设置，且 `fgetc()` 应返回 EOF。如果发生错误，流的错误指示符应被设置，`fgetc()` 应返回 EOF，并应设置 `errno` 以指示错误。

## ERRORS

如果需要读取数据，`fgetc()` 函数将失败：

- **[EAGAIN]** 为 `stream` 底层的文件描述符设置了 O_NONBLOCK 标志，线程将在 `fgetc()` 操作中被延迟。

- **[EBADF]** `stream` 底层的文件描述符不是用于读取的有效文件描述符。

- **[EINTR]** 读取操作因接收到信号而终止，且没有数据传输。

- **[EIO]** 发生物理 I/O 错误，或者进程处于后台进程组中尝试从其控制终端读取，并且调用线程正在阻塞 SIGTTIN，或者进程正在忽略 SIGTTIN，或者进程的进程组是孤立的。此错误也可能因实现定义的原因而产生。

- **[EOVERFLOW]** 文件是常规文件，且尝试在或超过相应流关联的偏移量最大值处读取。

在以下情况下 `fgetc()` 函数可能失败：

- **[ENOMEM]** 可用存储空间不足。

- **[ENXIO]** 对不存在的设备发出请求，或者请求超出了设备的能力。

## EXAMPLES

无。

## APPLICATION USAGE

如果将 `fgetc()` 返回的整数值存储到 **char** 类型的变量中，然后与整数常量 EOF 进行比较，比较可能永远不会成功，因为 **char** 类型的变量在扩展为整数时的符号扩展是实现定义的。

必须使用 `ferror()` 或 `feof()` 函数来区分错误条件和文件结束条件。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- 2.5 标准 I/O 流
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feof.html)
- [`ferror()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ferror.html)
- [`fgets()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgets.html)
- [`fread()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fread.html)
- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- [`getchar()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [`ungetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ungetc.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 5
添加了大文件峰会扩展。

### Issue 6
标记了超出 ISO C 标准的扩展。

POSIX 实现的以下新要求来源于与单一 UNIX 规范的对齐：
- 添加了 [EIO] 和 [EOVERFLOW] 强制错误条件。
- 添加了 [ENOMEM] 和 [ENXIO] 可选错误条件。

为与 ISO/IEC 9899:1999 标准对齐进行了以下更改：
- 更新了 DESCRIPTION 以阐明输入流的文件结束指示符未设置时的行为。
- 更新了 RETURN VALUE 部分，指出应设置流的错误指示符。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/32，将 ERRORS 部分中的 [EAGAIN] 错误从"进程将被延迟"更新为"线程将被延迟"。

### Issue 7
应用了 Austin Group Interpretation 1003.1-2001 #051，更新了标记最后数据访问时间戳以进行更新的函数列表。

应用了 POSIX.1-2008，技术勘误表 1，XSH/TC1-2008/0129 [79] 和 XSH/TC1-2008/0130 [14]。

### Issue 8
应用了 Austin Group Defect 1330，移除了过时的接口。

应用了 Austin Group Defect 1624，更改了 RETURN VALUE 部分。