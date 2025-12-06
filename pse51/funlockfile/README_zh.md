# flockfile, ftrylockfile, funlockfile — stdio 锁定函数

## 概要

```c
#include <stdio.h>

void flockfile(FILE *file);
int ftrylockfile(FILE *file);
void funlockfile(FILE *file);
```

## 描述

这些函数应提供与标准 I/O 流相关联的锁的显式应用级锁定（参见 [2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)）。线程可以使用这些函数来界定作为单元执行的一系列 I/O 语句。

`flockfile()` 函数应为线程获取 (`FILE *`) 对象的所有权。

`ftrylockfile()` 函数在对象可用时为线程获取 (`FILE *`) 对象的所有权；`ftrylockfile()` 是 `flockfile()` 的非阻塞版本。

`funlockfile()` 函数应放弃授予线程的所有权。如果当前所有者之外的线程调用 `funlockfile()` 函数，则行为未定义。

这些函数的行为应表现为每个 (`FILE *`) 对象都有一个与之关联的锁计数。此计数在创建 (`FILE *`) 对象时隐式初始化为零。当计数为零时，(`FILE *`) 对象处于未锁定状态。当计数为正时，单个线程拥有 (`FILE *`) 对象。当调用 `flockfile()` 函数时，如果计数为零，或者计数为正且调用者拥有 (`FILE *`) 对象，则计数应递增。否则，调用线程应被挂起，等待计数返回零。每次调用 `funlockfile()` 都会递减计数。这允许匹配的 `flockfile()` 调用（或成功的 `ftrylockfile()` 调用）和 `funlockfile()` 调用可以嵌套。

## 返回值

`flockfile()` 和 `funlockfile()` 无返回值。

`ftrylockfile()` 函数成功时应返回零，非零值表示无法获取锁。

## 错误

未定义任何错误。

*以下各节为参考性内容。*

## 示例

无。

## 应用用法

使用这些函数的应用程序可能会受到优先级反转的影响，如 XBD [3.275 Priority Inversion](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html#tag_03_275) 中所述。

对 [`exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exit.html) 的调用可能会阻塞，直到锁定的流被解锁，因为拥有 (`FILE *`) 对象所有权的线程会阻止其他线程中所有引用该 (`FILE *`) 对象的函数调用（名称以 `_unlocked` 结尾的函数除外），包括对 [`exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exit.html) 的调用。

**注意：** `FILE` 锁不是文件锁（参见 XBD [3.143 File Lock](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html#tag_03_143)）。

## 基本原理

`flockfile()` 和 `funlockfile()` 函数为每个 `FILE` 提供一个正交的互斥锁。`ftrylockfile()` 函数提供获取 `FILE` 锁的非阻塞尝试，类似于 [`pthread_mutex_trylock()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_trylock.html)。

这些锁的行为应表现为与 `stdio` 内部用于线程安全的锁相同。这既提供了这些函数的线程安全性，而无需第二级内部锁定，又允许 `stdio` 中的函数用其他 `stdio` 函数来实现。

应用程序开发者和实现者应该意识到 `FILE` 对象存在潜在的死锁问题。例如，`stdio` 的行缓冲刷新语义（通过 `{_IOLBF}` 请求）要求某些输入操作有时会导致实现定义的行缓冲输出流的缓冲内容被刷新。如果两个线程各自持有对方 `FILE` 的锁，就会发生死锁。这种类型的死锁可以通过以一致的顺序获取 `FILE` 锁来避免。特别是，如果线程既要获取输入流的锁又要获取输出流的锁，通常可以通过先获取输入流锁再获取输出流锁来避免行缓冲输出流的死锁。

总之，与其他线程共享 `stdio` 流的线程可以使用 `flockfile()` 和 `funlockfile()` 来使单个线程执行的 I/O 序列保持捆绑。唯一需要使用 `flockfile()` 和 `funlockfile()` 的情况是提供作用域保护 `*_unlocked` 函数/宏的使用。这将成本/性能权衡移至最优点。

## 未来方向

无。

## 另请参见

[`exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exit.html#tag_17_130), [`getc_unlocked()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc_unlocked.html)

XBD [3.275 Priority Inversion](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html#tag_03_275), [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## 变更历史

### 首次发布于 Issue 5
包含此内容以与 POSIX 线程扩展对齐。

### Issue 6
这些函数被标记为线程安全函数选项的一部分。

### Issue 7
`flockfile()`、`ftrylockfile()` 和 `funlockfile()` 函数从线程安全函数选项移至基础规范。

应用 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0140 [118]。

应用 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0116 [611]。

### Issue 8
应用 Austin Group Defect 1118，阐明 `FILE` 锁不是文件锁。

应用 Austin Group Defect 1302，将部分文本替换为对 [2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05) 的引用。

*参考性文本结束。*

---
