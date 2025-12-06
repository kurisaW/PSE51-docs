# getc — 从流中获取一个字节

## 概要 (SYNOPSIS)

```c
#include <stdio.h>

int getc(FILE *stream);
```

## 描述 (DESCRIPTION)

`getc()` 函数应等价于 `fgetc()`，但区别在于：如果它被实现为宏（macro），可能会对 *stream* 参数进行多次求值，因此该参数永远不应是具有副作用的表达式。

## 返回值 (RETURN VALUE)

参考 `fgetc()`。

## 错误 (ERRORS)

参考 `fgetc()`。

---

*以下为补充信息章节。*

## 示例 (EXAMPLES)

无。

## 应用用法 (APPLICATION USAGE)

如果 `getc()` 返回的整数值被存储到 `char` 类型的变量中，然后再与整数常量 EOF 进行比较，该比较可能永远不会成功，因为 `char` 类型变量在扩展为整数时的符号扩展是由实现定义的（implementation-defined）。

由于 `getc()` 可能被实现为宏，它可能错误处理带有副作用的 *stream* 参数。特别是，`getc(*f_++)` 不一定能按预期工作。因此，在这种情况下，使用该函数之前应该先执行 "#undef getc"；也可以使用 `fgetc()` 代替。

## 基本原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

- 2.5 标准输入输出流 (Standard I/O Streams)
- `fgetc()`
- XBD `<stdio.h>`

## 变更历史 (CHANGE HISTORY)

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008 技术勘误 1，XSH/TC1-2008/0231 [14]。

---
