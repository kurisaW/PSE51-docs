# strrchr — 字符串扫描操作

## 概要

```c
#include <string.h>

char *strrchr(const char *s, int c);
```

## 描述

`strrchr()` 函数应定位 `s` 所指向的字符串中 `c`（转换为 `char` 类型）的最后一次出现。终止的 NUL 字符被视为字符串的一部分。

对于有效输入，`strrchr()` 函数不应更改 `errno` 的设置。

## 返回值

成功完成后，`strrchr()` 应返回一个指向该字节的指针，如果 `c` 不在字符串中出现，则返回空指针。

## 错误

未定义错误。

## 示例

### 获取文件的基础名称

以下示例使用 `strrchr()` 获取指向文件基础名称的指针。`strrchr()` 函数通过文件名称向后搜索，以找到 `name` 中的最后一个 '/' 字符。这个指针（加一）将指向文件的基础名称。

```c
#include <string.h>
...
const char *name;
char *basename;
...
basename = strrchr(name, '/') + 1;
...
```

## 应用用法

无。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`strchr()`](strchr.html)
- [`<string.h>`](basedefs/string.h.html)

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 8

应用了 Austin Group Defect 448，增加了一项要求：对于有效输入，`strrchr()` 不得更改 `errno` 的设置。

---
