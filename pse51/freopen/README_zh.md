# freopen

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

freopen — 打开流

## SYNOPSIS

```c
#include <stdio.h>

FILE *freopen(const char *restrict pathname,
              const char *restrict mode,
              FILE *restrict stream);
```

## DESCRIPTION

[Option Start] 除了"独占访问"要求（参见 fopen()）外，本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何其他冲突都是无意的。POSIX.1-2024 在除"独占访问"外的所有 freopen() 功能上遵循 ISO C 标准。[Option End]

freopen() 函数应首先尝试刷新与 stream 关联的流，如同调用 fflush(stream) 一样。未能成功刷新流应被忽略。如果 pathname 不是空指针，freopen() 应关闭与 stream 关联的任何文件描述符。未能成功关闭文件描述符应被忽略。流的错误和文件结束指示器应被清除。

freopen() 函数应打开路径名为 pathname 指向的字符串的文件，并将 stream 指向的流与其关联。mode 参数的使用方式应与 fopen() 中的相同。

无论后续打开是否成功，原始流都应被关闭。

如果 pathname 是空指针，freopen() 函数应尝试将流的模式更改为 mode 指定的模式，如同使用了当前与流关联的文件名一样。在这种情况下，如果对 freopen() 的调用成功，与流关联的文件描述符不需要被关闭。允许哪些模式更改（如果有）以及在什么情况下允许更改，是由实现定义的。

成功调用 freopen() 函数后，流的定向应被清除，[Option Start] 编码规则应被清除，[Option End] 并且关联的 mbstate_t 对象应被设置为描述初始转换状态。

[Option Start] 如果 pathname 不是空指针，或者 pathname 是空指针但指定的模式更改需要关闭并重新打开与流关联的文件描述符，则与重新打开的流关联的文件描述符应被分配并打开，如同通过使用为具有相同 mode 参数的 fopen() 指定的标志调用 open() 一样。[Option End]

## RETURN VALUE

成功完成后，freopen() 应返回 stream 的值。否则，应返回空指针，[Option Start] 并设置 errno 以指示错误。[Option End]

## ERRORS

freopen() 函数在以下情况下应失败：

- **[EACCES]**
  [Option Start] 路径前缀的某个组件的搜索权限被拒绝，或者文件存在且 mode 指定的权限被拒绝，或者文件不存在且要创建的文件的父目录的写权限被拒绝。[Option End]

- **[EBADF]**
  [Option Start] 当 pathname 是空指针时，流底层的基础文件描述符不是有效的文件描述符。[Option End]

- **[EILSEQ]**
  [Option Start] mode 参数以 w 或 a 开始，文件之前不存在，并且最后一个路径名组件不是可移植的文件名，无法在目标目录中创建。[Option End]

- **[EEXIST]**
  [Option Start] mode 参数以 w 或 a 开始并包含 x，但文件已存在。[Option End]

- **[EINTR]**
  [Option Start] 在 freopen() 执行期间捕获到信号。[Option End]

- **[EISDIR]**
  [Option Start] 命名的文件是目录，且 mode 要求写访问。[Option End]

- **[ELOOP]**
  [Option Start] 在解析 pathname 参数期间遇到符号链接循环。[Option End]

- **[EMFILE]**
  [Option Start] 进程可用的所有文件描述符当前都已打开。[Option End]

- **[ENAMETOOLONG]**
  [Option Start] 路径名的某个组件的长度超过 {NAME_MAX}。[Option End]

- **[ENFILE]**
  [Option Start] 系统中当前打开的文件数已达到最大允许数量。[Option End]

- **[ENOENT]**
  [Option Start] mode 字符串以 'r' 开始且 pathname 的某个组件没有命名现有文件，或者 mode 以 'w' 或 'a' 开始且 pathname 的路径前缀的某个组件没有命名现有文件，或者 pathname 是空字符串。[Option End]

- **[ENOENT] 或 [ENOTDIR]**
  [Option Start] pathname 参数包含至少一个非 <slash> 字符并以一个或多个尾随 <slash> 字符结尾。如果去掉尾随 <slash> 字符的 pathname 会命名现有文件，则不应出现 [ENOENT] 错误。[Option End]

- **[ENOSPC]**
  [Option Start] 包含新文件的目录或文件系统无法扩展，文件不存在，且本应创建该文件。[Option End]

- **[ENOTDIR]**
  [Option Start] 路径前缀的某个组件命名了现有文件，该文件既不是目录也不是指向目录的符号链接，或者 pathname 参数包含至少一个非 <slash> 字符并以一个或多个尾随 <slash> 字符结尾，且最后一个路径名组件命名了现有文件，该文件既不是目录也不是指向目录的符号链接。[Option End]

- **[ENXIO]**
  [Option Start] 命名的文件是字符特殊文件或块特殊文件，并且与此特殊文件关联的设备不存在。[Option End]

- **[EOVERFLOW]**
  [Option Start] 命名的文件是常规文件，且文件大小无法在 off_t 类型的对象中正确表示。[Option End]

- **[EROFS]**
  [Option Start] 命名的文件位于只读文件系统上，且 mode 要求写访问。[Option End]

freopen() 函数在以下情况下可能失败：

- **[EBADF]**
  [Option Start] 当 pathname 是空指针时，打开流底层文件描述符的模式不支持请求的模式。[Option End]

- **[EINVAL]**
  [Option Start] mode 参数的值无效。[Option End]

- **[ELOOP]**
  [Option Start] 在解析 pathname 参数期间遇到超过 {SYMLOOP_MAX} 个符号链接。[Option End]

- **[ENAMETOOLONG]**
  [Option Start] 路径名的长度超过 {PATH_MAX}，或者符号链接的路径名解析产生的中间结果长度超过 {PATH_MAX}。[Option End]

- **[ENOMEM]**
  [Option Start] 可用存储空间不足。[Option End]

- **[ENXIO]**
  [Option Start] 对不存在的设备发出请求，或者请求超出了设备的功能范围。[Option End]

- **[ETXTBSY]**
  [Option Start] 文件是正在执行的纯过程（共享文本）文件，且 mode 要求写访问。[Option End]

---

*以下各节是提供信息的。*

## EXAMPLES

### 将标准输出重定向到文件

以下示例将所有标准输出记录到 /tmp/logfile 文件中。

```c
#include <stdio.h>
...
FILE *fp;
...
fp = freopen ("/tmp/logfile", "a+", stdout);
...
```

## APPLICATION USAGE

freopen() 函数通常用于将与 stdin、stdout 和 stderr 关联的预打开流附加到其他文件。

由于实现不要求在 pathname 参数为 NULL 时支持任何流模式更改，可移植的应用程序不能依赖使用 freopen() 来更改流模式，不鼓励使用此功能。此功能最初被添加到 ISO C 标准是为了便于将 stdin 和 stdout 更改为二进制模式。由于 mode 中的 'b' 字符在 POSIX 系统上没有效果，在 POSIX 应用程序中不需要使用此功能。但是，即使 'b' 被忽略，成功调用 freopen(NULL, "wb", stdout) 确实有效果。特别是，对于常规文件，它会截断文件并将流的文件位置指示器设置为文件的开头。这些副作用可能是 ISO/IEC 9899:1999 标准（以及当前标准）中指定此功能的方式的意外后果，但除非或直到 ISO C 标准被更改，成功调用 freopen(NULL, "wb", stdout) 的应用程序在符合标准的系统上的情况下会以意想不到的方式表现，例如：

```
{ appl file1; appl file2; } > file3
```

这将导致 file3 仅包含第二次调用 appl 的输出。

另请参见 fopen() 的 APPLICATION USAGE。

## RATIONALE

参见 fopen() 的 RATIONALE。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- 2.5 标准 I/O 流
- fclose()
- fdopen()
- fflush()
- fmemopen()
- fopen()
- mbsinit()
- open()
- open_memstream()
- <stdio.h>

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

DESCRIPTION 更新为指示流的定向在成功调用 freopen() 函数后被清除，流的转换状态被设置为初始转换状态。

添加了大文件峰会扩展。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源自与单一 UNIX 规范的对齐：

- 在 DESCRIPTION 中，添加了文本以指示在打开文件描述中设置偏移量最大值。此更改是为了支持大文件。
- 在 ERRORS 部分中，添加了 [EOVERFLOW] 条件。此更改是为了支持大文件。
- 添加了 [ELOOP] 强制错误条件。
- 添加了第二个 [ENAMETOOLONG] 作为可选错误条件。
- 添加了 [EINVAL]、[ENOMEM]、[ENXIO] 和 [ETXTBSY] 可选错误条件。

为与 ISO/IEC 9899:1999 标准对齐进行了以下更改：

- 更新了 freopen() 原型。
- 更新了 DESCRIPTION。

强制 [ELOOP] 错误条件的措辞已更新，并添加了第二个可选 [ELOOP] 错误条件。

关于关闭失败的 DESCRIPTION 已更新，将"文件"更改为"文件描述符"。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/40，在 DESCRIPTION 中添加以下句子："在这种情况下，如果对 freopen() 的调用成功，与流关联的文件描述符不需要被关闭。"。

应用 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/41，向 ERRORS 部分添加了强制 [EBADF] 错误和可选 [EBADF] 错误。

### Issue 7

应用 Austin Group Interpretation 1003.1-2001 #043，阐明 freopen() 函数按照 open() 的方式分配文件描述符。

应用 Austin Group Interpretation 1003.1-2001 #143。

应用 Austin Group Interpretation 1003.1-2001 #159，阐明对打开文件描述上设置的标志的要求。

应用 SD5-XBD-ERN-4，更改 [EMFILE] 错误的定义。

应用 SD5-XSH-ERN-150 和 SD5-XSH-ERN-219。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0181 [291,433], XSH/TC1-2008/0182 [146,433], XSH/TC1-2008/0183 [324], 和 XSH/TC1-2008/0184 [14]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0134 [822]。

### Issue 8

应用 Austin Group Defect 293，添加 [EILSEQ] 错误。

应用 Austin Group Defect 411，添加 e 和 x 模式字符串字符。

应用 Austin Group Defect 1200，更正 [ELOOP] 错误中的参数名称。

应用 Austin Group Defect 1302，将此函数与 ISO/IEC 9899:2018 标准对齐。

*信息性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 The IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved