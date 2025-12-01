# flockfile, ftrylockfile, funlockfile — stdio 锁定函数

## SYNOPSIS

```c
#include <stdio.h>

void flockfile(FILE *file);
int ftrylockfile(FILE *file);
void funlockfile(FILE *file);
```

## DESCRIPTION

这些函数为与标准 I/O 流（参见 2.5 标准 I/O 流）相关联的锁提供明确的应用程序级锁定。线程可以使用这些函数来划分作为一个单元执行的 I/O 语句序列。

`flockfile()` 函数应为线程获取 (`FILE *`) 对象的所有权。

`ftrylockfile()` 函数应在对象可用时为线程获取 (`FILE *`) 对象的所有权；`ftrylockfile()` 是 `flockfile()` 的非阻塞版本。

`funlockfile()` 函数应放弃授予线程的所有权。如果当前所有者以外的线程调用 `funlockfile()` 函数，行为是未定义的。

这些函数的行为应表现为每个 (`FILE *`) 对象都关联有一个锁计数器。当 (`FILE *`) 对象创建时，该计数器被隐式初始化为零。当计数器为零时，(`FILE *`) 对象处于未锁定状态。当计数器为正数时，单个线程拥有 (`FILE *`) 对象。当调用 `flockfile()` 函数时，如果计数器为零，或者计数器为正数且调用者拥有 (`FILE *`) 对象，则计数器应递增。否则，调用线程应被挂起，等待计数器返回零。每次调用 `funlockfile()` 都应递减计数器。这使得匹配的 `flockfile()`（或成功的 `ftrylockfile()`）调用和 `funlockfile()` 调用可以嵌套。

## RETURN VALUE

`flockfile()` 和 `funlockfile()` 没有返回值。

`ftrylockfile()` 函数成功时应返回零，非零值表示无法获取锁。

## ERRORS

未定义错误。

## APPLICATION USAGE

使用这些函数的应用程序可能会受到优先级反转的影响，如 XBD 3.275 优先级反转中所述。

对 `exit()` 的调用可能会阻塞，直到锁定的流被解锁，因为拥有 (`FILE*`) 对象所有权的线程会阻止其他线程引用该 (`FILE*`) 对象的所有函数调用（名称以 `_unlocked` 结尾的函数除外），包括对 `exit()` 的调用。

注意：FILE 锁不是文件锁（参见 XBD 3.143 文件锁）。

## RATIONALE

`flockfile()` 和 `funlockfile()` 函数为每个 FILE 提供一个正交互斥锁。`ftrylockfile()` 函数提供获取 FILE 锁的非阻塞尝试，类似于 `pthread_mutex_trylock()`。

这些锁的行为表现为与 stdio 内部用于线程安全的锁相同。这既提供了这些函数的线程安全，又不需要第二级内部锁定，并允许 stdio 中的函数用其他 stdio 函数来实现。

应用程序开发者和实现者应该注意，FILE 对象存在潜在的死锁问题。例如，stdio 的行缓冲刷新语义（通过 `{_IOLBF}` 请求）要求某些输入操作有时导致实现定义的行缓冲输出流的缓冲内容被刷新。如果两个线程各自持有对方 FILE 的锁，就会发生死锁。通过以一致的顺序获取 FILE 锁可以避免这种类型的死锁。特别是，如果线程既要获取输入流又要获取输出流的锁，通常可以通过在获取输出流锁之前获取输入流锁来避免行缓冲输出流死锁。

总之，与其他线程共享 stdio 流的线程可以使用 `flockfile()` 和 `funlockfile()` 来保持单个线程执行的 I/O 序列捆绑在一起。需要使用 `flockfile()` 和 `funlockfile()` 的唯一情况是提供保护 `*_unlocked` 函数/宏使用的作用域。这会将成本/性能权衡移动到最佳点。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `exit()`
- `getc_unlocked()`
- XBD 3.275 优先级反转
- `<stdio.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5
为了与 POSIX 线程扩展对齐而包含。

### Issue 6
这些函数被标记为线程安全函数选项的一部分。

### Issue 7
`flockfile()`、`ftrylockfile()` 和 `funlockfile()` 函数从线程安全函数选项移动到基础标准。

应用 POSIX.1-2008 技术勘误表 1，XSH/TC1-2008/0140 [118]。

应用 POSIX.1-2008 技术勘误表 2，XSH/TC2-2008/0116 [611]。

### Issue 8
应用 Austin Group 缺陷 1118，明确说明 FILE 锁不是文件锁。

应用 Austin Group 缺陷 1302，用对 2.5 标准 I/O 流的引用替换部分文本。