# ctime — 将时间值转换为日期和时间字符串

## 概要 (SYNOPSIS)

```c
#include <time.h>

char *ctime(const time_t *clock);
```

## 描述 (DESCRIPTION)

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`ctime()` 函数应将 `clock` 指向的时间值（表示自纪元（Epoch）以来的秒数）转换为本地时间的字符串形式。它应等价于：

```c
asctime(localtime(clock))
```

`asctime()`、`ctime()`、`gmtime()` 和 `localtime()` 函数应在两个静态对象之一中返回值：一个分解时间结构和一个 `char` 数组。执行任何返回指向这些对象类型之一的指针的函数可能会覆盖从之前对其中任何函数的调用所返回的值所指向的任何同类型对象中的信息。

`ctime()` 函数不需要是线程安全的；但是，`ctime()` 应避免与除自身、`asctime()`、`gmtime()` 和 `localtime()` 之外的所有函数发生数据竞争。

## 返回值 (RETURN VALUE)

`ctime()` 函数应返回以该分解时间作为参数调用 `asctime()` 所返回的指针。

## 错误 (ERRORS)

未定义错误。

---

## 示例 (EXAMPLES)

无。

## 应用程序用法 (APPLICATION USAGE)

此函数仅用于与旧实现兼容。如果生成的字符串过长，它具有未定义行为，因此应不鼓励使用此函数。在未检测到输出字符串长度溢出的实现上，可能会以导致应用程序失败的方式溢出输出缓冲区，或者可能造成系统安全违规。此外，此函数不支持本地化的日期和时间格式。为避免这些问题，应用程序应使用 `strftime()` 从分解时间生成字符串。

分解时间结构的值可以通过调用 `gmtime()` 或 `localtime()` 来获得。

尝试对纪元之前的时间或 9999 年之后的时间使用 `ctime()` 会产生未定义结果。请参考 `asctime()`。

## 原理 (RATIONALE)

标准开发者决定将 `ctime()` 函数标记为过时，尽管它包含在 ISO C 标准中，原因是可能存在缓冲区溢出。ISO C 标准也提供了 `strftime()` 函数，可用于避免这些问题。

## 未来方向 (FUTURE DIRECTIONS)

此函数可能在将来的版本中被删除，但不会在从 ISO C 标准中删除之前删除。

## 参见 (SEE ALSO)

- `asctime()`
- `clock()`
- `difftime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`
- XBD `<time.h>`

## 变更历史 (CHANGE HISTORY)

### 首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 5

- 以前在应用程序用法章节中的规范性文本被移至描述章节。
- 为与 POSIX 线程扩展对齐，包含了 `ctime_r()` 函数。
- 在描述章节中添加了说明 `ctime()` 函数不需要可重入的注释。

### Issue 6

- 超出 ISO C 标准的扩展被标记。
- 在描述章节中，关于可重入性的注释被扩展以覆盖线程安全性。
- 应用程序用法章节被更新，包含关于线程安全函数及其避免可能使用静态数据区域的注释。

### Issue 7

- 应用了 Austin Group 解释 1003.1-2001 #156。
- 应用了 SD5-XSH-ERN-25，更新了应用程序用法。
- 应用了 Austin Group 解释 1003.1-2001 #053，将这些函数标记为过时。
- `ctime_r()` 函数从线程安全函数选项移至基础。
- 应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0066 [321,428]。
- 应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0075 [664]。

### Issue 8

- 应用了 Austin Group 缺陷 1302，使此函数与 ISO/IEC 9899:2018 标准对齐。
- 应用了 Austin Group 缺陷 1330，更改了未来方向章节。
- 应用了 Austin Group 缺陷 1376，从源自 ISO C 标准的某些文本中移除了 CX 着色，并更新以匹配 ISO C 标准。
- 应用了 Austin Group 缺陷 1410，移除了 `ctime_r()` 函数。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*