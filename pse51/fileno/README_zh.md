# fileno — 将流指针映射为文件描述符

## 概要

```c
#include <stdio.h>

int fileno(FILE *stream);
```

## 描述

`fileno()` 函数应返回与 `stream` 参数指向的流相关联的整数文件描述符。

## 返回值

成功完成后，`fileno()` 应返回与 `stream` 相关联的文件描述符的整数值。否则，应返回值 -1 并设置 `errno` 以指示错误。

## 错误

在以下情况下，`fileno()` 函数应失败：

- **[EBADF]**
  流与文件无关联。

在以下情况下，`fileno()` 函数可能失���：

- **[EBADF]**
  `stream` 底层的文件描述符不是有效的文件描述符。

## 示例

无。

## 应用程序用法

无。

## 原理说明

如果没有规范说明哪些文件描述符与这些流相关联，应用程序就无法为通过 `fork()` 和 `exec` 启动的另一个应用程序设置流。特别是，将无法编写可移植版本的 `sh` 命令解释器（尽管可能有其他约束会阻止这种可移植性）。

## 未来方向

无。

## 参见

- *2.5.1 文件描述符与标准 I/O 流的交互*
- `dirfd()`
- `fdopen()`
- `fopen()`
- `stdin`
- **XBD** `<stdio.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

对 POSIX 实现的以下新要求源于与单一 UNIX 规范的对齐：

- 添加了 [EBADF] 可选错误条件。

### Issue 7

应用了 SD5-XBD-ERN-99，更改了 [EBADF] 错误的定义。

应用了 POSIX.1-2008 Technical Corrigendum 2，XSH/TC2-2008/0115 [589]。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*