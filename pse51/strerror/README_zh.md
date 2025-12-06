# strerror, strerror_l, strerror_r — 获取错误消息字符串

## SYNOPSIS（概要）

```c
#include <string.h>

char *strerror(int errnum);

/* XSI 扩展 */
char *strerror_l(int errnum, locale_t locale);
int strerror_r(int errnum, char *strerrbuf, size_t buflen);
```

## DESCRIPTION（描述）

对于 strerror()：
- 本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 标准遵循 ISO C 标准。

strerror() 函数应将 errnum 中的错误编号映射为依赖于语言环境的错误消息字符串，并返回指向该字符串的指针。通常，errnum 的值来自 errno，但 strerror() 应将任何 int 类型的值映射为消息。

应用程序不应修改返回的字符串。后续的 strerror() 调用，或同一线程中后续的 strerror_l() 调用，可能会使返回的字符串指针失效或覆盖字符串内容。如果调用线程终止，返回的指针和字符串内容也可能失效。

该字符串可能被同一线程中后续的 strerror_l() 调用覆盖。

strerror() 返回的错误消息字符串的内容应由当前语言环境中 LC_MESSAGES 类别的设置决定。

实现的行为应如同 POSIX.1-2024 标准卷中定义的任何函数都不调用 strerror()。

如果成功，strerror() 和 strerror_l() 函数不应更改 errno 的设置。

由于没有保留返回值来指示 strerror() 的错误，希望检查错误情况的应用程序应将 errno 设置为 0，然后调用 strerror()，再检查 errno。类似地，由于 strerror_l() 需要为某些错误返回字符串，希望检查所有错误情况的应用程序应将 errno 设置为 0，然后调用 strerror_l()，再检查 errno。

strerror() 函数不需要是线程安全的；但是，strerror() 应避免与所有其他函数发生数据竞争。

strerror_l() 函数应将 errnum 中的错误编号映射为由 locale 表示的语言环境中的依赖于语言环境的错误消息字符串，并返回指向该字符串的指针。

strerror_r() 函数应将 errnum 中的错误编号映射为依赖于语言环境的错误消息字符串，并应在 strerrbuf 指向的缓冲区（长度为 buflen）中返回该字符串。

如果 errnum 的值是有效的错误编号，消息字符串应指示发生了什么错误；如果 errnum 的值是零，消息字符串应为空字符串或指示没有发生错误；否则，如果这些函数成功完成，消息字符串应指示发生了未知错误。

如果传递给 strerror_l() 的 locale 参数是特殊的语言环境对象 LC_GLOBAL_LOCALE 或不是有效的语言环境对象句柄，则行为未定义。

## RETURN VALUE（返回值）

完成时，无论成功与否，strerror() 都应返回指向生成的消息字符串的指针。错误时可能设置 errno，但没有保留返回值来指示错误。

成功完成时，strerror_l() 应返回指向生成的消息字符串的指针。如果 errnum 不是有效的错误编号，可能将 errno 设置为 [EINVAL]，但仍应返回指向消息字符串的指针。如果发生任何其他错误，应设置 errno 来指示错误，并返回空指针。

成功完成时，strerror_r() 应返回 0。否则，应返回错误编号来指示错误。

## ERRORS（错误）

这些函数可能在以下情况下失败：

- **EINVAL** - errnum 的值既不是有效的错误编号也不是零。

strerror_r() 函数应在以下情况下失败：

- **ERANGE** - 通过 strerrbuf 和 buflen 提供的存储空间不足以包含生成的消息字符串。

## EXAMPLES（示例）

无。

## APPLICATION USAGE（应用程序使用）

历史上在某些实现中，调用 perror() 会覆盖 strerror() 返回的指针指向的字符串。这样的实现不符合 ISO C 标准；但是，如果应用程序开发者希望其应用程序可移植到此类实现，应注意这种行为。

应用程序应使用 strerror_l() 而不是 strerror() 或 strerror_r()，以避免线程安全问题和某些实现中这些函数可能的替代（非符合）版本。

## RATIONALE（基本原理）

strerror_l() 函数需要是线程安全的，从而消除了对等效于 strerror_r() 函数的需求。

本标准的早期版本没有明确要求 strerror() 和 strerror_r() 返回的错误消息字符串提供有关错误的任何信息。本版本的标准要求任何成功完成都要提供有意义的消息。

由于没有保留返回值来指示 strerror() 错误，但所有调用（无论成功与否）都必须返回指向消息字符串的指针，错误时 strerror() 可以返回指向空字符串的指针或指向可以打印的有意义字符串的指针。

请注意，[EINVAL] 错误条件是"可能失败"的错误。如果提供了无效的错误编号作为 errnum 的值，应用程序应准备好处理以下任何情况：

1. **错误（没有有意义的消息）**：errno 设置为 [EINVAL]，返回值是指向空字符串的指针。

2. **成功完成**：errno 未更改，返回值指向类似"unknown error"或"error number xxx"（其中 xxx 是 errnum 的值）的字符串。

3. **#1 和 #2 的组合**：errno 设置为 [EINVAL]，返回值指向类似"unknown error"或"error number xxx"（其中 xxx 是 errnum 的值）的字符串。由于应用程序经常使用 strerror() 的返回值作为 fprintf() 等函数的参数（而不检查返回值），并且应用程序无法解析错误消息字符串来确定 errnum 是否表示有效的错误编号，鼓励实现采用 #3。类似地，当 errnum 的值不是有效的错误编号时，鼓励 strerror_r() 返回 [EINVAL] 并在 strerrbuf 指向的缓冲区中放入类似"unknown error"或"error number xxx"的字符串。

此外，对于除零外的任何大小，鼓励在因 [ERANGE] 失败时对 strerrbuf 进行空终止。

一些应用程序依赖于能够在调用没有保留值来指示错误的函数之前将 errno 设置为 0，然后调用 strerror(errno) 来检测是否发生错误（因为 errno 更改）或指示成功（因为 errno 保持为零）。这种使用模式要求 strerror(0) 成功并提供有用的结果。标准的前一个版本没有指定当 errnum 为零时的行为。

## FUTURE DIRECTIONS（未来方向）

无。

## SEE ALSO（参见）

- [perror()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/perror.html)
- [<string.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## CHANGE HISTORY（变更历史）

首次发布于 Issue 3。

### Issue 5

DESCRIPTION 更新为指示如果函数成功则不更改 errno。

在 DESCRIPTION 中添加了说明 strerror() 函数不需要可重入的注释。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 在 RETURN VALUE 部分，添加了可能设置 errno 的事实。
- 添加了 [EINVAL] 可选错误条件。

规范性文本更新为避免对应用程序要求使用"必须"一词。

根据 IEEE PASC Interpretation 1003.1c #39 添加了 strerror_r() 函数。

strerror_r() 函数被标记为线程安全函数选项的一部分。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #072，更新了 ERRORS 部分。

应用了 Austin Group Interpretation 1003.1-2001 #156。

应用了 Austin Group Interpretation 1003.1-2001 #187，阐明了当生成的错误消息为空字符串时的行为。

应用了 SD5-XSH-ERN-191，更新了 APPLICATION USAGE 部分。

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 strerror_l() 函数。

strerror_r() 函数从线程安全函数选项移至 Base。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0595 [75], XSH/TC1-2008/0596 [447], XSH/TC1-2008/0597 [382,428], XSH/TC1-2008/0598 [283], XSH/TC1-2008/0599 [382,428], XSH/TC1-2008/0600 [283], 和 XSH/TC1-2008/0601 [382,428]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0339 [656]。

### Issue 8

应用了 Austin Group Defect 398，将 [ERANGE] 错误从"可能失败"更改为"应失败"。

应用了 Austin Group Defect 655，更改了 APPLICATION USAGE 部分。

应用了 Austin Group Defect 1302，使 strerror() 函数与 ISO/IEC 9899:2018 标准对齐。

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved