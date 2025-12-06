# fdopen

## 概要

```c
[CX] #include <stdio.h>

FILE *fdopen(int fildes, const char *mode);
```

## 描述

`fdopen()` 函数应将一个流与文件描述符关联。

`mode` 参数指向一个对 `fopen()` 有效的字符串。如果该字符串以以下字符之一开头，那么流应按照指定的方式与 `fildes` 关联。否则，行为是未定义的。

### 模式字符

**'r'**
- 如果 `mode` 包含 '+'，关联的流应打开用于更新（读写）
- 否则，流应打开仅用于读取
- 如果 `fildes` 引用的打开文件描述设置了 O_APPEND，它应保持设置状态

**'w'**
- 如果 `mode` 包含 '+'，关联的流应打开用于更新（读写）
- 否则，流应打开仅用于写入
- 文件不应被 `fdopen()` 调用截断
- 如果 `fildes` 引用的打开文件描述设置了 O_APPEND，它应保持设置状态

**'a'**
- 如果 `mode` 包含 '+'，关联的流应打开用于更新（读写）
- 否则，流应打开仅用于写入
- 如果 `fildes` 引用的打开文件描述清除了 O_APPEND，是否通过 `fdopen()` 调用设置 O_APPEND 或保持清除状态是未指定的

### 附加模式选项

- `mode` 中存在 'x' 应没有效果
- 如果不存在 'e'，`fildes` 的 FD_CLOEXEC 标志应保持不变；如果存在 'e'，应通过 `fdopen()` 调用设置
- 实现可能支持 `mode` 参数的附加值

### 要求

应用程序应确保通过 `mode` 参数表示的流模式被 `fildes` 引用的打开文件描述的文件访问模式所允许。

与新流关联的文件位置指示器设置为与文件描述符关联的文件偏移量指示的位置。

流的错误和文件结束指示器应被清除。`fdopen()` 函数可能导致基础文件的最后数据访问时间戳被标记为更新。

[SHM] 如果 `fildes` 引用共享内存对象，`fdopen()` 函数的结果是未指定的。

[TYM] 如果 `fildes` 引用类型化内存对象，`fdopen()` 函数的结果是未指定的。

`fdopen()` 函数应保留之前为对应于 `fildes` 的打开文件描述设置的最大偏移量。

## 返回值

成功完成后，`fdopen()` 应返回指向流的指针；否则，应返回空指针并设置 `errno` 以指示错误。

## 错误

`fdopen()` 函数应在以下情况下失败：

- **[EMFILE]** {STREAM_MAX} 流当前在调用进程中打开。

`fdopen()` 函数可能在以下情况下失败：

- **[EBADF]** `fildes` 参数不是有效的文件描述符。
- **[EINVAL]** `mode` 参数不是有效的模式。
- **[EMFILE]** {FOPEN_MAX} 流当前在调用进程中打开。
- **[ENOMEM]** 空间不足以分配缓冲区。

---

*以下部分是资料性的。*

## 示例

无。

## 应用程序用法

文件描述符通过 `open()`、`dup()`、`creat()` 或 `pipe()` 等调用获得，这些调用打开文件但不返回流。

## 原理

文件描述符可能通过 `open()`、`creat()`、`pipe()`、`dup()`、`fcntl()` 或 `socket()` 获得；通过 `fork()`、`posix_spawn()` 或 `exec()` 继承；或者通过其他方式获得。

`fdopen()` 和 `fopen()` 的 `mode` 参数含义不同。对于 `fdopen()`，写入（'w'）模式不能创建或截断文件，追加（'a'）模式不能创建文件。为了与 `fopen()` 保持一致，允许在 `mode` 参数中包含 'b'；'b' 对结果流没有影响。关于指定追加（'a'）模式是否在 O_APPEND 标志清除时设置该标志，实现有所不同，但鼓励这样做。由于 `fdopen()` 不创建文件，'x' 模式修饰符被静默忽略。'e' 模式修饰符对于 `fdopen()` 并非严格必要，因为当它不存在时 FD_CLOEXEC 不能被改变；但是，这里对其进行标准化，因为该修饰符对于避免使用 `freopen()` 的多线程应用程序中的数据竞争是必要的，并且一致性要求所有接受 `mode` 字符串的函数应该允许相同的字符串集合。

## 未来方向

无。

## 另见

- **2.5.1 文件描述符与标准 I/O 流的交互**
- [`fclose()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fclose.html)
- [`fmemopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fmemopen.html)
- [`fopen()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html)
- [`open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/open.html)
- [`open_memstream()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/open_memstream.html)
- [`posix_spawn()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/posix_spawn.html)
- [`socket()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/socket.html)
- XBD **<stdio.h>**

## 变更历史

### 在 Issue 1 中首次发布。源自 SVID 的 Issue 1。

### Issue 5

- 更新了描述以与 POSIX 实时扩展对齐。
- 添加了大文件峰会扩展。

### Issue 6

POSIX 实现的以下新要求源自与单一 UNIX 规范的对齐：

- 在描述中，`mode` 参数的使用和设置被更改为包含二进制流。
- 在描述中，添加了大文件支持的文本，以指示在打开文件描述中设置最大偏移量。
- 添加了错误部分中识别的所有错误。
- 在描述中，添加了 `fdopen()` 函数可能导致 `st_atime` 更新的文本。

为与 IEEE P1003.1a 草案标准对齐进行了以下更改：

- 添加了说明，确保模式与打开文件描述符兼容是应用程序的责任。

通过指定对于类型化内存对象 `fdopen()` 结果是未指定的，更新了描述以与 IEEE Std 1003.1j-2000 对齐。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/30，对原理进行了更正。

### Issue 7

- 应用了 SD5-XSH-ERN-149，添加了 {STREAM_MAX} [EMFILE] 错误条件。
- 进行了与细粒度时间戳支持相关的更改。
- 应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0121 [409]。

### Issue 8

- 应用了 Austin Group 缺陷 411 和 1526，更改了 `mode` 参数的要求。

---

*资料性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利