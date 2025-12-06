# memchr

## 概要

```c
#include <string.h>

void *memchr(const void *s, int c, size_t n);
```

## 描述

`memchr()` 函数应在 `s` 指向的初始 `n` 个字节（每个字节都解释为 `unsigned char`）中定位 `c`（转换为 `unsigned char`）的首次出现。

实现在行为上应表现为顺序读取字节，并在找到匹配字节后立即停止。

对于有效输入，`memchr()` 函数不应改变 `errno` 的设置。

## 返回值

`memchr()` 函数应返回指向所定位字节的指针，如果未找到该字节，则返回空指针。

## 错误

未定义错误。

---

## 示例

无。

## 应用用法

无。

## 基本原理

无。

## 未来方向

无.

## 另请参阅

`<string.h>`

## 更改历史

### 首次发布于 Issue 1。派生自 SVID 的 Issue 1。

### Issue 7

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0374 [110]。

### Issue 8

应用了 Austin Group Defect 448，增加了 `memchr()` 对于有效输入不改变 `errno` 设置的要求。

应用了 Austin Group Defect 1302，使此函数与 ISO/IEC 9899:2018 标准保持一致。

---
