# strerror, strerror_l, strerror_r — 获取错误消息字符串

## 概要 (SYNOPSIS)

```c
#include <string.h>

char *strerror(int errnum);

[CX] char *strerror_l(int errnum, locale_t locale);
int strerror_r(int errnum, char *strerrbuf, size_t buflen);
```

## 描述 (DESCRIPTION)

对于 `strerror()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`strerror()` 函数应将 `errnum` 中的错误号映射到依赖于区域设置的错误消息字符串，并应返回指向该字符串的指针。通常，`errnum` 的值来自 `errno`，但 `strerror()` 应将任何 `int` 类型的值映射到消息。

应用程序不应修改返回的字符串。[CX] 返回的字符串指针可能被后续对 `strerror()` 的调用所无效化，或者字符串内容可能被覆盖，[CX] 或者在同一线程中后续对 `strerror_l()` 的调用所覆盖。如果调用线程终止，返回的指针和字符串内容也可能被无效化。

[CX] 字符串可能被同一线程中后续对 `strerror_l()` 的调用所覆盖。

`strerror()` 返回的错误消息字符串的内容应由当前区域设置中的 `LC_MESSAGES` 类别设置决定。

实现的行为应如同本 POSIX.1-2024 卷中定义的任何函数都不调用 `strerror()`。

[CX] 如果成功，`strerror()` 和 `strerror_l()` 函数不应更改 `errno` 的设置。

由于没有保留返回值来指示 `strerror()` 的错误，希望检查错误情况的应用程序应将 `errno` 设置为 0，然后调用 `strerror()`，然后检查 `errno`。类似地，由于 `strerror_l()` 被要求为某些错误返回字符串，希望检查所有错误情况的应用程序应将 `errno` 设置为 0，然后调用 `strerror_l()`，然后检查 `errno`。

`strerror()` 函数不需要是线程安全的；但是，`strerror()` 应避免与所有其他函数发生数据竞争。

[CX] `strerror_l()` 函数应将 `errnum` 中的错误号映射到由 `locale` 表示的区域设置中依赖于区域设置的错误消息字符串，并应返回指向该字符串的指针。

`strerror_r()` 函数应将 `errnum` 中的错误号映射到依赖于区域设置的错误消息字符串，并应将字符串返回到由 `strerrbuf` 指向的缓冲区中，长度为 `buflen`。

[CX] 如果 `errnum` 的值是有效的错误号，消息字符串应指示发生了什么错误；如果 `errnum` 的值为零，消息字符串应为空字符串或指示没有发生错误；否则，如果这些函数成功完成，消息字符串应指示发生了未知错误。

[CX] 如果 `strerror_l()` 的 `locale` 参数是特殊的区域设置对象 LC_GLOBAL_LOCALE 或不是有效的区域设置对象句柄，则行为未定义。

## 返回值 (RETURN VALUE)

完成时，无论成功与否，`strerror()` 都应返回指向生成的消息字符串的指针。[CX] 出错时 `errno` 可能被设置，但没有保留返回值来指示错误。

成功完成时，`strerror_l()` 应返回指向生成的消息字符串的指针。如果 `errnum` 不是有效的错误号，`errno` 可能被设置为 [EINVAL]，但仍应返回指向消息字符串的指针。如果发生任何其他错误，`errno` 应被设置为指示错误，并返回空指针。

成功完成时，`strerror_r()` 应返回 0。否则，应返回错误号以指示错误。

## 错误 (ERRORS)

这些函数可能在以下情况下失败：

- **[EINVAL]** [CX] `errnum` 的值既不是有效的错误号也不是零。

`strerror_r()` 函数应在以下情况下失败：

- **[ERANGE]** [CX] 通过 `strerrbuf` 和 `buflen` 提供的存储空间不足以包含生成的消息字符串。

## 示例 (EXAMPLES)

无。

## 应用程序用法 (APPLICATION USAGE)

历史上，在某些实现中，对 `perror()` 的调用会覆盖 `strerror()` 返回的指针所指向的字符串。这样的实现不符合 ISO C 标准；然而，如果应用程序开发人员希望他们的应用程序可以移植到这样的实现，应该注意这种行为。

应用程序应使用 `strerror_l()` 而不是 `strerror()` 或 `strerror_r()`，以避免线程安全问题以及在某些实现中这些函数的替代（非符合）版本。

## 基本原理 (RATIONALE)

要求 `strerror_l()` 函数是线程安全的，从而消除了对等同于 `strerror_r()` 函数的需求。

本标准的早期版本没有明确要求 `strerror()` 和 `strerror_r()` 返回的错误消息字符串提供有关错误的任何信息。本版本的标准要求任何成功完成时都要提供有意义的消息。

由于没有保留返回值来指示 `strerror()` 错误，但所有调用（无论成功与否）都必须返回指向消息字符串的指针，出错时 `strerror()` 可以返回指向空字符串的指针或返回指向可打印的有意义字符串的指针。

请注意，[EINVAL] 错误条件是一个可能失败（may fail）的错误。如果提供了无效的错误号作为 `errnum` 的值，应用程序应准备处理以下任何情况：

1. 错误（没有有意义的消息）：`errno` 被设置为 [EINVAL]，返回值是指向空字符串的指针。
2. 成功完成：`errno` 未更改，返回值指向类似 "unknown error" 或 "error number xxx"（其中 `xxx` 是 `errnum` 的值）的字符串。
3. #1 和 #2 的组合：`errno` 被设置为 [EINVAL]，返回值指向类似 "unknown error" 或 "error number xxx"（其中 `xxx` 是 `errnum` 的值）的字符串。由于应用程序经常使用 `strerror()` 的返回值作为 `fprintf()` 等函数的参数（而不检查返回值），并且应用程序无法解析错误消息字符串来确定 `errnum` 是否代表有效的错误号，鼓励实现实现 #3。类似地，鼓励当 `errnum` 的值不是有效的错误号时，让 `strerror_r()` 返回 [EINVAL] 并在 `strerrbuf` 指向的缓冲区中放入类似 "unknown error" 或 "error number xxx" 的字符串。

此外，鼓励在因 [ERANGE] 失败时（对于 `buflen` 非零的任何大小）将 `strerrbuf` 以 null 终止。

一些应用程序依赖于能够在调用没有保留值来指示错误的函数之前将 `errno` 设置为 0，然后调用 `strerror(errno)` 来检测是否发生了错误（因为 `errno` 更改了）或指示成功（因为 `errno` 保持为零）。这种使用模式要求 `strerror(0)` 成功并产生有用的结果。标准的早期版本没有指定当 `errnum` 为零时的行为。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

- `perror()`
- XBD `<string.h>`

## 变更历史 (CHANGE HISTORY)

首次在 Issue 3 中发布。

### Issue 5

DESCRIPTION 已更新，以指示如果函数成功则不更改 `errno`。

在 DESCRIPTION 中添加了一条注释，指出 `strerror()` 函数不需要是可重入的。

### Issue 6

标记了超出 ISO C 标准的扩展���

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 在 RETURN VALUE 部分，添加了 `errno` 可能被设置的事实。
- 添加了 [EINVAL] 可选错误条件。

规范性文本已更新，以避免对应用程序要求使用 "must" 一词。

为响应 IEEE PASC Interpretation 1003.1c #39，添加了 `strerror_r()` 函数。

`strerror_r()` 函数被标记为线程安全函数选项的一部分。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #072，更新了 ERRORS 部分。

应用了 Austin Group Interpretation 1003.1-2001 #156。

应用了 Austin Group Interpretation 1003.1-2001 #187，阐明了当生成的错误消息为空字符串时的行为。

应用了 SD5-XSH-ERN-191，更新了 APPLICATION USAGE 部分。

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `strerror_l()` 函数。

`strerror_r()` 函数从线程安全函数选项移动到 Base。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0595 [75], XSH/TC1-2008/0596 [447], XSH/TC1-2008/0597 [382,428], XSH/TC1-2008/0598 [283], XSH/TC1-2008/0599 [382,428], XSH/TC1-2008/0600 [283], 和 XSH/TC1-2008/0601 [382,428]。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0339 [656]。

### Issue 8

应用了 Austin Group Defect 398，将 [ERANGE] 错误从 "可能失败"（may fail）更改为 "应失败"（shall fail）。

应用了 Austin Group Defect 655，更改了 APPLICATION USAGE 部分。

应用了 Austin Group Defect 1302，使 `strerror()` 函数与 ISO/IEC 9899:2018 标准对齐。

---
