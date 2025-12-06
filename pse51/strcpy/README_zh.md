# strcpy

## 概要

```c
#include <string.h>

[CX] char *stpcpy(char *restrict s1, const char *restrict s2);

char *strcpy(char *restrict s1, const char *restrict s2);
```

## 描述

对于 `strcpy()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 卷服从 ISO C 标准。

[CX] `stpcpy()` 和 `strcpy()` 函数应将由 `s2` 指向的字符串（包括终止的 NUL 字符）复制到由 `s1` 指向的数组中。

如果在重叠的对象之间进行复制，则行为未定义。

[CX] `strcpy()` 和 `stpcpy()` 函数在有效输入时不应更改 `errno` 的设置。

## 返回值

[CX] `stpcpy()` 函数应返回指向复制到 `s1` 缓冲区中的终止 NUL 字符的指针。

`strcpy()` 函数应返回 `s1`。

没有保留任何返回值来指示错误。

## 错误

未定义任何错误。

---

*以下章节为参考信息。*

## 示例

### 在单个缓冲区中构造多部分消息

```c
#include <string.h>
#include <stdio.h>

int
main (void)
{
    char buffer[10];
    char *name = buffer;

    name = stpcpy (stpcpy (stpcpy (name, "ice"),"-"), "cream");
    puts (buffer);
    return 0;
}
```

### 初始化字符串

以下示例将字符串 "----------" 复制到 `permstring` 变量中。

```c
#include <string.h>
...
static char permstring[11];
...
strcpy(permstring, "----------");
...
```

### 存储键和数据

以下示例使用 `malloc()` 为键分配空间，然后使用 `strcpy()` 将键放置在那里。然后使用 `malloc()` 为数据分配空间，并使用 `strcpy()` 将数据放置在那里。（用户定义的函数 `dbfree()` 释放先前分配给 `struct element *` 类型数组的内存。）

```c
#include <string.h>
#include <stdlib.h>
#include <stdio.h>
...
/* 用于读取数据并存储的结构体。 */
struct element {
    char *key;
    char *data;
};

struct element *tbl, *curtbl;
char *key, *data;
int count;
...
void dbfree(struct element *, int);
...
if ((curtbl->key = malloc(strlen(key) + 1)) == NULL) {
    perror("malloc"); dbfree(tbl, count); return NULL;
}
strcpy(curtbl->key, key);

if ((curtbl->data = malloc(strlen(data) + 1)) == NULL) {
    perror("malloc"); free(curtbl->key); dbfree(tbl, count); return NULL;
}
strcpy(curtbl->data, data);
...
```

## 应用程序使用

字符移动在不同的实现中执行方式不同。因此，重叠移动可能会产生意外结果。

此版本与 ISO C 标准保持一致；这不会影响与 XPG3 应用程序的兼容性。此函数的可靠错误检测从未得到保证。

## 原理

无。

## 未来方向

无。

## 另请参阅

- `strncpy()`
- `wcscpy()`
- XBD `<string.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

`strcpy()` 原型已更新，以与 ISO/IEC 9899:1999 标准保持一致。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 1 中添加了 `stpcpy()` 函数。

### Issue 8

应用了 Austin Group Defect 448，增加了 `strcpy()` 和 `stpcpy()` 在有效输入时不更改 `errno` 设置的要求。

应用了 Austin Group Defect 1787，更改了 NAME 部分。

---

*参考信息文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利