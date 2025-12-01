# getenv, secure_getenv — 获取环境变量的值

## 概要

```c
#include <stdlib.h>

char *getenv(const char *name);

[CX] char *secure_getenv(const char *name);
```

## 描述

[CX] 本参考页描述的功能与ISO C标准保持一致。此处描述的要求与ISO C标准之间的任何冲突都是无意的。本POSIX.1-2024版本遵循ISO C标准。

`getenv()` 函数应在调用进程的环境中搜索环境变量 *name*（见XBD 8. 环境变量），如果存在则返回指向该环境变量值的指针。如果找不到指定的环境变量，则应返回空指针。应用程序应确保不修改 `getenv()` 函数指向的字符串，[CX] 除非该字符串是先前通过给 `environ` [XSI] 赋值或使用 `putenv()` 而放入环境中的可修改对象的一部分。

[CX] `getenv()` 返回的指针应指向 `environ` 指向的环境数据中的字符串。

> **注意：**
> 这个要求是对ISO C标准的扩展，ISO C标准允许 `getenv()` 将数据复制到内部缓冲区。

`secure_getenv()` 函数应等同于 `getenv()`，但如果调用进程不满足以下所有安全条件，则应返回空指针：

1. 调用进程的有效用户ID和实际用户ID在程序启动时相等。
2. 调用进程的有效组ID和实际组ID在程序启动时相等。
3. 实现定义的额外安全条件。

## 返回值

成功完成后，`getenv()` 应返回指向包含指定 *name* 值的字符串的指针。如果在调用进程的环境中找不到指定的 *name*，则应返回空指针。

[CX] 成功完成后，`secure_getenv()` 应返回指向包含指定 *name* 值的字符串的指针。如果在调用进程的环境中找不到指定的 *name*，或者调用进程不满足描述中列出的安全条件，则应返回空指针。

## 错误

未定义任何错误。

---

## 示例

### 获取环境变量的值

以下示例获取HOME环境变量的值。

```c
#include <stdlib.h>
...
const char *name = "HOME";
char *value;

value = getenv(name);
```

---

## 应用程序使用

无。

---

## 原理说明

`clearenv()` 函数曾被考虑但被拒绝。`putenv()` 函数现已包含在内，以与Single UNIX Specification保持一致。

本标准的某些早期版本不要求 `getenv()` 是线程安全的，因为它被允许返回指向内部缓冲区的值。然而，ISO C标准允许的这种行为在POSIX.1中不再被允许。POSIX.1要求数据环境通过 `environ[]` 可用，因此没有理由不让 `getenv()` 返回指向实际数据的指针而不是副本。因此，现在要求 `getenv()` 是线程安全的（除非另一个线程修改了环境）。

符合要求的应用程序被要求不直接修改 `environ` 指向的指针，而是只使用 `setenv()`、`unsetenv()` 和 `putenv()` 函数，或对 `environ` 本身赋值来操作进程环境。这个约束允许实现正确管理它分配的内存。这使得实现在调用 `unsetenv()` 时可以释放已分配给存储在 `environ` 中的字符串（以及可能指向它们的指针）的任何空间。C运行时启动过程（调用 `main()` 并可能初始化 `environ` 的过程）也可以初始化一个标志，表示环境尚未复制到分配的存储空间，或者单独的表尚未初始化。如果应用程序通过给 `environ` 赋新值切换到完整的新环境，这可以被 `getenv()`、`setenv()`、`unsetenv()` 或 `putenv()` 检测到，实现此时可以基于新环境重新初始化。（这可能包括将环境字符串复制到新数组并让 `environ` 指向它。）

事实上，为了获得更高的 `getenv()` 性能，不提供 `putenv()` 的实现也可以在单独的数据结构中维护环境的单独副本，该结构可以更快地搜索（如索引哈希表或二叉树），并在调用 `setenv()` 或 `unsetenv()` 时同时更新它和 `environ` 上的线性列表。在提供 `putenv()` 的实现上，这样的副本可能仍然有价值，但需要考虑应用程序可以直接修改使用 `putenv()` 添加的环境字符串内容的事实。例如，如果通过搜索副本找到的环境字符串是使用 `putenv()` 添加的，实现需要检查 `environ` 中的字符串是否仍然具有相同的名称（和值，如果副本包含值），并且每当搜索副本没有匹配项时，实现需要搜索 `environ` 中每个使用 `putenv()` 添加的环境字符串，以防它们中有任何更改了名称现在匹配。因此，每次使用 `putenv()` 添加到环境都会降低拥有副本的速度优势。

对于拥有大量环境变量的应用程序，`getenv()` 的性能可能很重要。通常，这样的应用程序将环境用作用户可配置参数的资源数据库。这些变量位于用户shell环境中的事实通常意味着任何其他使用环境变量的程序（如尝试使用 `COLUMNS` 的 `ls`，或者几乎所有实用程序（`LANG`、`LC_ALL` 等））都会因为线性搜索变量而同样变慢。

维护单独的数据结构，甚至管理其消耗内存的实现目前不是必需的，因为被认为会减少不想改变其历史实现的实现者之间的共识。

---

## 未来方向

无。

---

## 参见

- [`exec()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`putenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putenv.html)
- [`setenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setenv.html)
- [`unsetenv()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/unsetenv.html)

XBD 8. 环境变量, `<stdlib.h>`

---

## 变更历史

首次在Issue 1中发布。源自SVID的Issue 1。

### Issue 5

先前在应用程序使用部分的规范性文本被移至返回值部分。

在描述中添加了一个注释，指出此函数不需要是可重入的。

### Issue 6

进行了以下更改以与IEEE P1003.1a草案标准保持一致：

- 添加了对新的 `setenv()` 和 `unsetenv()` 函数的引用。

规范性文本被更新以避免对应用程序要求使用"must"一词。

### Issue 7

应用了Austin Group解释1003.1-2001 #062，澄清了调用 `putenv()` 也可能导致字符串被覆盖。

应用了Austin Group解释1003.1-2001 #148，添加了未来方向。

应用了Austin Group解释1003.1-2001 #156。

应用了POSIX.1-2008技术勘误1，XSH/TC1-2008/0238 [75,428]、XSH/TC1-2008/0239 [167]和XSH/TC1-2008/0240 [167]。

应用了POSIX.1-2008技术勘误2，XSH/TC2-2008/0157 [656]。

### Issue 8

应用了Austin Group缺陷188和1394，将 `getenv()` 更改为线程安全的。

应用了Austin Group缺陷922，添加了 `secure_getenv()` 函数。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 IEEE和The Open Group*