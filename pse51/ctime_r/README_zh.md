# ctime_r — 将时间值转换为日期和时间字符串（已移除）

## SYNOPSIS

```c
#include <time.h>

char *ctime(const time_t *clock);
```

## DESCRIPTION

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本标准遵循 ISO C 标准。

`ctime()` 函数应将 `clock` 指向的时间（表示自纪元以来以秒为单位的时间）转换为字符串形式的本地时间。它应等价于：

```c
asctime(localtime(clock))
```

`asctime()`、`ctime()`、`gmtime()` 和 `localtime()` 函数应在两个静态对象之一中返回值：一个分解时间结构和一个 `char` 数组。执行任何返回指向这些对象类型之一指针的函数可能会覆盖任何先前调用其中任何函数所返回的值所指向的同类对象中的信息。

`ctime()` 函数不需要是线程安全的；但是，`ctime()` 应避免与除自身、`asctime()`、`gmtime()` 和 `localtime()` 之外的所有函数发生数据竞争。

## RETURN VALUE

`ctime()` 函数应返回 `asctime()` 以该分解时间为参数时返回的指针。

## ERRORS

未定义错误。

---

## EXAMPLES

无。

## APPLICATION USAGE

此函数仅为了与较旧的实现兼容而包含。如果结果字符串过长，其行为未定义，因此应 discourage 使用此函数。在不检测输出字符串长度溢出的实现上，可能会以导致应用程序失败或可能造成系统安全违规的方式溢出输出缓冲区。此外，此函数不支持本地化的日期和时间格式。为避免这些问题，应用程序应使用 `strftime()` 从分解时间生成字符串。

分解时间结构的值可以通过调用 `gmtime()` 或 `localtime()` 获得。

尝试对纪元之前的时间或 9999 年之后的时间使用 `ctime()` 会产生未定义的结果。请参考 `asctime()`。

## RATIONALE

标准开发者决定将 `ctime()` 函数标记为过时，尽管它在 ISO C 标准中，这是由于存在缓冲区溢出的可能性。ISO C 标准也提供了 `strftime()` 函数，可用于避免这些问题。

## FUTURE DIRECTIONS

此函数可能在将来的版本中被移除，但不会在从 ISO C 标准中移除之前移除。

## SEE ALSO

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

## CHANGE HISTORY

### 首次发布于 Issue 1
源自 SVID 的 Issue 1。

### Issue 5
- 之前位于 APPLICATION USAGE 部分的规范性文本移至 DESCRIPTION。
- 包含 `ctime_r()` 函数以与 POSIX 线程扩展对齐。
- 在 DESCRIPTION 中添加了指示 `ctime()` 函数不需要是可重入的说明。

### Issue 6
- 标记了超出 ISO C 标准的扩展。
- 在 DESCRIPTION 中，关于可重入性的说明扩展为涵盖线程安全性。
- 更新了 APPLICATION USAGE 部分，以包含关于线程安全函数及其避免可能使用静态数据区域的说明。

### Issue 7
- 应用了 Austin Group Interpretation 1003.1-2001 #156。
- 应用了 SD5-XSH-ERN-25，更新了 APPLICATION USAGE。
- 应用了 Austin Group Interpretation 1003.1-2001 #053，将这些函数标记为过时。
- `ctime_r()` 函数从线程安全函数选项移至基础部分。
- 应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0066 [321,428]。
- 应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0075 [664]。

### Issue 8
- 应用了 Austin Group Defect 1302，使此函数与 ISO/IEC 9899:2018 标准对齐。
- 应用了 Austin Group Defect 1330，更改了 FUTURE DIRECTIONS 部分。
- 应用了 Austin Group Defect 1376，从源自 ISO C 标准的某些文本中移除 CX 着色并更新以匹配 ISO C 标准。
- **应用了 Austin Group Defect 1410，移除了 `ctime_r()` 函数。**

---
