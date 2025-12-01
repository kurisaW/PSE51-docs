# difftime — 计算两个日历时间值之间的差值

## 概要 (SYNOPSIS)

```c
#include <time.h>

double difftime(time_t time1, time_t time0);
```

## 描述 (DESCRIPTION)

本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。本 POSIX.1-2024 卷遵循 ISO C 标准。

`difftime()` 函数应计算两个日历时间（由 `time()` 返回）之间的差值：`time1 - time0`。

## 返回值 (RETURN VALUE)

`difftime()` 函数应返回以 `double` 类型表示的秒数差值。

## 错误 (ERRORS)

未定义任何错误。

---

*以下章节为信息性内容。*

## 示例 (EXAMPLES)

无。

## 应用用法 (APPLICATION USAGE)

无。

## 基本原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 参见 (SEE ALSO)

- `asctime()`
- `clock()`
- `ctime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `time()`

[XBD `<time.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/time.h.html)

## 变更历史 (CHANGE HISTORY)

首次发布于 Issue 4。源自 ISO C 标准。

---

*信息性文本结束。*

**来源**: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024
版权 © 2001-2024 IEEE 和 The Open Group