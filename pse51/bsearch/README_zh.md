# bsearch — 二分搜索已排序的表

The Open Group Base Specifications Issue 8
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

---

## 名称

`bsearch` — 二分搜索已排序的表

## 概要

```c
#include <stdlib.h>

void *bsearch(const void *key, const void *base, size_t nel,
              size_t width, int (*compar)(const void *, const void *));
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`bsearch()` 函数应搜索由 `nel` 个对象组成的数组，该数组的初始元素由 `base` 指向，寻找与 `key` 指向的对象匹配的元素。数组中每个元素的大小由 `width` 指定。如果 `nel` 参数值为零，则不应调用 `compar` 指向的比较函数，且不会找到匹配项。

`compar` 指向的比较函数应使用两个参数调用，这两个参数分别指向 `key` 对象和数组元素，顺序如此。

应用程序应确保 `compar` 指向的比较函数不改变数组的内容。实现可以在调用比较函数之间重新排序数组的元素，但不应改变任何单个元素的内容。

实现应确保第一个参数始终是指向键的指针。

当相同的对象（由 width 字节组成，无论它们在数组中的当前位置如何）被多次传递给比较函数时，结果应彼此一致。也就是说，相同的对象应始终以相同的方式与键进行比较。

应用程序应确保函数返回小于、等于或大于 0 的整数，如果 `key` 对象被认为分别小于、匹配或大于数组元素。应用程序应确保数组由所有比较小于键的对象、所有比较等于键的对象和所有比较大于键的对象组成，按此顺序排列。

## 返回值

`bsearch()` 函数应返回指向数组中匹配成员的指针，如果未找到匹配项则返回空指针。如果有两个或更多成员比较相等，则返回哪个成员是未指定的。

## 错误

未定义错误。

---

*以下各节为提供信息的内容。*

## 示例

下面的示例搜索一个表，该表包含指向由字符串及其长度组成的节点的指针。该表按照每个条目指向的节点中的字符串按字母顺序排序。

下面的代码片段读取字符串，要么找到相应的节点并打印出字符串及其长度，要么打印错误消息。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TABSIZE    1000


struct node {                  /* 这些存储在表中。 */
    char *string;
    int length;
};
struct node table[TABSIZE];    /* 要搜索的表。 */
    .
    .
    .
{
    struct node *node_ptr, node;
    /* 比较 2 个节点的例程。 */
    int node_compare(const void *, const void *);
    .
    .
    .
    while (scanf("%ms", &node.string) != EOF) {
        node_ptr = (struct node *)bsearch((void *)(&node),
               (void *)table, TABSIZE,
               sizeof(struct node), node_compare);
        if (node_ptr != NULL) {
            (void)printf("string = %20s, length = %d\n",
                node_ptr->string, node_ptr->length);
        } else {
            (void)printf("not found: %s\n", node.string);
        }
        free(node.string);
    }
}
/*
    这个例程基于字符串字段的字母顺序比较两个节点。
*/
int
node_compare(const void *node1, const void *node2)
{
    return strcoll(((const struct node *)node1)->string,
        ((const struct node *)node2)->string);
}
```

## 应用程序用法

指向键和表基址元素的指针应为元素指针类型。

比较函数不需要比较每个字节，因此除了被比较的值之外，元素中可以包含任意数据。

在实践中，数组通常根据比较函数进行排序。

## 基本原理

要求比较函数的第二个参数（此后称为 `p`）是指向数组元素的指针，这意味着对于每次调用，以下所有表达式都不为零：

```
( (char *)p - (char *)base ) % width == 0
(char *)p >= (char *)base
(char *)p < (char *)base + nel * width
```

## 未来方向

无。

## 另请参见

- [`hcreate()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/hcreate.html)
- [`lsearch()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/lsearch.html)
- [`qsort()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/qsort.html)
- [`tdelete()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/tdelete.html)

XBD [`<stdlib.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html)

## 变更历史

**首次发布于 Issue 1。** 源自 SVID 的 Issue 1。

### Issue 6

规范性文本已更新，避免对应用程序要求使用"必须"一词。

应用 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/11，向描述中添加第一个非阴影段落的最后一句，以及以下三个段落。基本原理部分也已更新。这些更改是为了与 ISO C 标准保持一致。

### Issue 7

示例部分已修订。

应用 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0051 [756]。

*信息性文本结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved