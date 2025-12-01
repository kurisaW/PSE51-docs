# rand, srand — 伪随机数生成器

## SYNOPSIS

```c
#include <stdlib.h>

int rand(void);
void srand(unsigned seed);
```

## DESCRIPTION

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`rand()` 函数应计算 [0,{RAND_MAX}] [XSI] 范围内的伪随机整数序列，周期至少为 2^32。

`rand()` 函数不需要是线程安全的；但是，`rand()` 应避免与非线程安全的伪随机序列生成函数之外的所有函数发生数据竞争。

`srand()` 函数使用参数作为新的伪随机数序列的种子，该序列将由后续对 `rand()` 的调用返回。如果随后使用相同的种子值调用 `srand()`，则应重复伪随机数序列。如果在任何对 `srand()` 的调用之前调用 `rand()`，则将生成与首次使用种子值 1 调用 `srand()` 时相同的序列。

`srand()` 函数不需要是线程安全的；但是，`srand()` 应避免与非线程安全的伪随机序列生成函数之外的所有函数发生数据竞争。

实现的行为应如同 POSIX.1-2024 本卷中定义的任何函数都不调用 `rand()` 或 `srand()`。

## RETURN VALUE

`rand()` 函数应返回序列中的下一个伪随机数。

`srand()` 函数不应返回值。

## ERRORS

未定义错误。

## EXAMPLES

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

## APPLICATION USAGE

当需要满足非平凡要求（包括安全性）时，应避免使用这些函数，除非使用 `getentropy()` 进行种子初始化。

`drand48()` 和 `random()` 函数提供了更为精密的伪随机数生成器。

## RATIONALE

ISO C 标准的 `rand()` 和 `srand()` 函数允许所有线程共享每个进程的伪随机流。这两个函数不需要更改，但必须有互斥机制防止两个线程同时访问随机数生成器时的干扰。

关于 `rand()`，在多线程程序中可能需要两种不同的行为：

1. 所有调用 `rand()` 的线程共享的单个每进程伪随机数序列
2. 每个调用 `rand()` 的线程的不同伪随机数序列

这是通过修改的线程安全函数提供的，该函数基于种子值是对整个进程全局还是对每个线程本地。

这并未解决 `rand()` 函数实现中已知的缺陷，这些缺陷已通过维护更多状态来处理。实际上，这指定了有缺陷函数的新线程安全形式。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `drand48()`
- `getentropy()`
- `initstate()`
- `<stdlib.h>`

## CHANGE HISTORY

### 首次发布于 Issue 1
源自 SVID 的 Issue 1。

### Issue 5
- 包含 `rand_r()` 函数以与 POSIX 线程扩展保持一致。
- 在 DESCRIPTION 中添加了说明 `rand()` 函数不需要是可重入的注释。

### Issue 6
- 标记了超出 ISO C 标准的扩展。
- 将 `rand_r()` 函数标记为线程安全函数选项的一部分。

### Issue 7
- 应用了 Austin Group Interpretation 1003.1-2001 #156。
- 将 `rand_r()` 函数标记为过时。
- 应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0301 [743]。

### Issue 8
- 应用了 Austin Group Defect 1134，添加了 `getentropy()`。
- 应用了 Austin Group Defect 1302，将这些函数与 ISO/IEC 9899:2018 标准保持一致。
- 应用了 Austin Group Defect 1330，移除了过时接口。

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 The IEEE 的商标。*
*版权所有 © 2001-2024 The IEEE and The Open Group，保留所有权利*