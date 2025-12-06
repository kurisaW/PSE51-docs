# puts

## 概要 (SYNOPSIS)

```c
#include <stdio.h>

int puts(const char *s);
```

## 描述 (DESCRIPTION)

`puts()` 函数应将由 `s` 指向的字符串写入标准输出流 `stdout`，后跟一个换行符。终止的空字节不会被写入。

在 `puts()` 成功执行与在同一流上成功完成下一次 `fflush()` 或 `fclose()` 调用，或 `exit()` 或 `abort()` 调用之间，应标记文件以更新最后数据修改和最后文件状态更改时间戳。

## 返回值 (RETURN VALUE)

成功完成后，`puts()` 应返回一个非负数。否则，它应返回 EOF，应设置流的错误指示器，并应设置 `errno` 以指示错误。

## 错误 (ERRORS)

参考 `fputc()`。

---

*以下内容为参考信息。*

## 示例 (EXAMPLES)

### 打印到标准输出

以下示例获取当前时间，使用 `localtime()` 和 `asctime()` 将其转换为字符串，并使用 `puts()` 将其打印到标准输出。然后它打印正在等待的事件的分钟数。

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
printf("The time is ");
puts(asctime(localtime(&now)));
printf("There are %d minutes to the event.\n",
    minutes_to_event);
...
```

## 应用程序用法 (APPLICATION USAGE)

`puts()` 函数会附加换行符，而 `fputs()` 不会。

POSIX.1-2024 卷要求成功完成时仅返回一个非负整数。对于此要求，至少有三种已知的不同实现约定：

* 返回常数值。
* 返回最后写入的字符。
* 返回写入的字节数。请注意，对于长度超过 {INT_MAX} 字节的字符串，不能遵循此实现约定，因为该值无法在函数的返回类型中表示。为了向后兼容，对于长度不超过 {INT_MAX} 字节的字符串，实现可以返回字节数，对于所有更长的字符串，返回 {INT_MAX}。

## 原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

[2.5 标准 I/O 流](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05), [fopen()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fopen.html), [fputs()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fputs.html), [putc()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putc.html)

XBD [<stdio.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 更改历史 (CHANGE HISTORY)

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 7

进行了与支持细粒度时间戳相关的更改。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0476 [174,412] 和 XSH/TC1-2008/0477 [14]。

参考文本结束。

---

**UNIX® 是 The Open Group 的注册商标。**
**POSIX™ 是 The IEEE 的商标。**
**版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利**