# fputs

## 名称

fputs — 向流写入字符串

## 概要

```c
#include <stdio.h>

int fputs(const char *restrict s, FILE *restrict stream);
```

## 描述

`fputs()` 函数应将由 `s` 指向的以 null 结尾的字符串写入由 `stream` 指向的流中。终止的 null 字节不应被写入。

文件的数据最后修改时间戳和文件状态最后更改时间戳应在 `fputs()` 成功执行与下一次在同一流上成功完成 `fflush()` 或 `fclose()` 调用，或调用 `exit()` 或 `abort()` 之间被标记为更新。

## 返回值

成功完成时，`fputs()` 应返回一个非负数。否则，应返回 EOF，为流设置错误指示符，并设置 `errno` 以指示错误。

## 错误

参考 `fputc()`。

## 示例

### 打印到标准输出

以下示例获取当前时间，使用 `localtime()` 和 `asctime()` 将其转换为字符串，并使用 `fputs()` 将其打印到标准输出。然后打印等待事件的分钟数。

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
printf("The time is ");
fputs(asctime(localtime(&now)), stdout);
printf("There are still %d minutes to the event.\n",
    minutes_to_event);
...
```

## 应用用法

`puts()` 函数会附加一个 `<newline>`（换行符），而 `fputs()` 不会。

POSIX.1-2024 这一卷要求成功完成时仅返回一个非负整数。对于此要求，至少有三种已知的不同的实现约定：

* 返回一个常量值。
* 返回最后写入的字符。
* 返回写入的字节数。请注意，对于长度超过 {INT_MAX} 字节的字符串，此实现约定无法遵循，因为该值无法在函数的返回类型中表示。为了向后兼容，对于长度不超过 {INT_MAX} 字节的字符串，实现可以返回字节数，对于所有更长的字符串，返回 {INT_MAX}。

## 基本原理

`fputs()` 函数是在引用的 *The C Programming Language* 中指定了源代码的函数之一。在最初版本中，该函数没有定义返回值，但许多实际的实现会作为副作用返回最后写入字符的值，因为这是累加器中剩余的用作返回值的值。在该书的第二版中，将根据 `ferror()` 的返回值返回固定值 0 或 EOF；然而，为了与现有实现兼容，多个实现在成功时会返回一个表示最后写入字节的正值。

## 未来方向

无。

## 另请参见

* 2.5 标准 I/O 流
* `fopen()`
* `putc()`
* `puts()`
* XBD `<stdio.h>`

## 更改历史

首次在 Issue 1 中发布。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

更新了 `fputs()` 原型以与 ISO/IEC 9899:1999 标准对齐。

### Issue 7

进行了与支持细粒度时间戳相关的更改。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0170 [174,412], XSH/TC1-2008/0171 [412], 和 XSH/TC1-2008/0172 [14]。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 The IEEE and The Open Group*