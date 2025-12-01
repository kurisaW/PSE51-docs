# rand, rand_r, srand - 伪随机数生成器

## 概要

```c
#include <stdlib.h>

int rand(void);

[OB CX] int rand_r(unsigned *seed);

void srand(unsigned seed);
```

## 描述

对于 `rand()` 和 `srand()`：[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2017 本卷遵循 ISO C 标准。

`rand()` 函数应计算一系列 [0, {RAND_MAX}] [XSI] 范围内的伪随机整数，周期至少为 2^32。

[CX] `rand()` 函数不必是线程安全的。

[OB CX] `rand_r()` 函数应计算一系列 [0, {RAND_MAX}] 范围内的伪随机整数。（{RAND_MAX} 宏的值应至少为 32767。）

如果使用相同的初始值调用 `rand_r()` 指向的 `seed` 对象，并且在该对象在连续的返回和 `rand_r()` 调用之间不被修改，则应生成相同的序列。

`srand()` 函数使用参数作为新的伪随机数序列的种子，该序列将由后续的 `rand()` 调用返回。如果随后使用相同的种子值调用 `srand()`，伪随机数序列将被重复。如果在任何 `srand()` 调用之前调用 `rand()`，将生成与第一次使用种子值 1 调用 `srand()` 时相同的序列。

实现的行为应该像是 POSIX.1-2017 本卷中定义的任何函数都不会调用 `rand()` 或 `srand()`。

## 返回值

`rand()` 函数应返回序列中的下一个伪随机数。

[OB CX] `rand_r()` 函数应返回一个伪随机整数。

`srand()` 函数不应返回值。

## 错误

未定义错误。

## 示例

### 生成伪随机数序列

以下示例演示如何生成伪随机数序列。

```c
#include <stdio.h>
#include <stdlib.h>
...
    long count, i;
    char *keystr;
    int elementlen, len;
    char c;
...
/* 初始化随机数生成器。 */
    srand(1);

    /* 仅使用小写字符创建键 */
    len = 0;
    for (i=0; i<count; i++) {
        while (len < elementlen) {
            c = (char) (rand() % 128);
            if (islower(c))
                keystr[len++] = c;
        }

        keystr[len] = '\0';
        printf("%s Element%0*ld\n", keystr, elementlen, i);
        len = 0;
    }
```

### 在不同机器上生成相同序列

以下代码定义了一对函数，可以集成到希望确保在不同机器上生成相同数字序列的应用程序中。

```c
static unsigned long next = 1;

int myrand(void)  /* 假设 RAND_MAX 为 32767。 */
{
    next = next * 1103515245 + 12345;
    return((unsigned)(next/65536) % 32768);
}

void mysrand(unsigned seed)
{
    next = seed;
}
```

## 应用用法

`drand48()` 和 `random()` 函数提供了更加精密的伪随机数生成器。

在函数调用之间可以传递的状态数量的限制意味着 `rand_r()` 函数永远无法以满足伪随机数生成器所有要求的方式实现。

当需要满足非平凡要求（包括安全性）时，应避免使用这些函数。

## 原理

ISO C 标准的 `rand()` 和 `srand()` 函数允许所有线程共享的每进程伪随机流。这两个函数不需要改变，但必须存在互斥机制，以防止两个线程同时访问随机数生成器时的干扰。

关于 `rand()`，在多线程程序中可能需要两种不同的行为：

1. 一个单一的每进程伪随机数序列，由所有调用 `rand()` 的线程共享
2. 每个调用 `rand()` 的线程都有不同的伪随机数序列

这是通过修改的线程安全函数提供的，具体取决于种子值是对整个进程是全局的，还是对每个线程是局部的。

这并没有解决 `rand()` 函数实现中已知的缺陷，这些缺陷通过维护更多状态来处理。实际上，这指定了有缺陷函数的新线程安全形式。

## 未来方向

`rand_r()` 函数可能在未来的版本中被移除。

## 参见

`drand48()`, `initstate()`

XBD `<stdlib.h>`

## 变更历史

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

为了与 POSIX 线程扩展保持一致，包含了 `rand_r()` 函数。

在 DESCRIPTION 中添加了指出 `rand()` 函数不必是可重入的注释。

### Issue 6

标记了超出 ISO C 标准的扩展。

将 `rand_r()` 函数标记为线程安全函数选项的一部分。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #156。

将 `rand_r()` 函数标记为已废弃。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0301 [743]。