# asctime_r — 将日期和时间转换为字符串（已移除）

## 状态

**已移除**：`asctime_r()` 函数已从 POSIX.1-2024 标准中移除（Issue 8，Austin Group Defect 1410）。

## 历史信息

本页面记录了 `asctime_r()` 函数的历史背景，该函数之前是 POSIX 标准的一部分，但现已被移除。

## 名称（历史）

> asctime_r — 将日期和时间转换为字符串（线程安全）

## 概要（历史）

```c
#include <time.h>

char *asctime_r(const struct tm *restrict timeptr, char *restrict buf);
```

## 描述

此参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 版本遵循 ISO C 标准。

`asctime_r()` 函数是 `asctime()` 的线程安全版本，旨在将 `timeptr` 指向的结构体中的分解时间转换为以下格式的字符串：

```
Sun Sep 16 01:03:52 1973\n\0
```

### 与 asctime() 的主要区别

- **线程安全性**：`asctime_r()` 是线程安全的，而 `asctime()` 不是
- **缓冲区管理**：`asctime_r()` 要求调用者提供缓冲区
- **静态数据避免**：`asctime_r()` 避免使用静态数据区域

## 历史背景

### 在 POSIX 版本中的演进

**Issue 5**：`asctime_r()` 函数被纳入以与 POSIX 线程扩展保持一致。

**Issue 6**：
- `asctime_r()` 函数被标记为线程安全函数选项的一部分
- 为与 ISO/IEC 9899:1999 标准保持一致，在 `asctime_r()` 原型中添加了 `restrict` 关键字
- 标记了超出 ISO C 标准的扩展
- 更新了应用程序使用部分，包含关于线程安全函数的说明

**Issue 7**：
- `asctime_r()` 函数从线程安全函数选项移至基础标准
- 应用了 Austin Group Interpretation 1003.1-2001 #053，将这些函数标记为过时

**Issue 8**：
- 应用了 Austin Group Defect 1410，**从标准中移除了 `asctime_r()` 函数**

## 移除理由

标准开发者决定将 `asctime()` 函数（并由此扩展到 `asctime_r()`）标记为过时，尽管它在 ISO C 标准中，但由于可能存在缓冲区溢出的问题。ISO C 标准也提供了 `strftime()` 函数，可以用来避免这些问题。

## 应用程序使用（历史）

此函数仅为与旧实现兼容而包含。如果结果字符串过长，它的行为是未定义的，因此应不鼓励使用此函数。

**推荐的替代方案**：应用程序应使用 `strftime()` 从分解时间生成字符串，以避免缓冲区溢出问题并支持本地化的日期和时间格式。

分解时间结构体的值可以通过调用 `gmtime()` 或 `localtime()` 获得。

## 未来方向

`asctime_r()` 函数已被移除，在未来的 POSIX 标准版本中将不可用。

## 另见

- [`asctime()`](asctime.html) - 非线程安全版本（也已过时）
- [`clock()`](clock.html)
- [`ctime()`](ctime.html)
- [`difftime()`](difftime.html)
- [`gmtime()`](gmtime.html)
- [`localtime()`](localtime.html)
- [`mktime()`](mktime.html)
- [`strftime()`](strftime.html) - **推荐的替代品**
- [`strptime()`](strptime.html)
- [`time()`](time.html)
- XBD [`<time.h>`](../basedefs/time.h.html)

## 变更历史

**首次发布**：Issue 1。源自 SVID 的 Issue 1。

**Issue 5**：
- 之前在应用程序使用部分中的规范性文本移至描述部分
- `asctime_r()` 函数被纳入以与 POSIX 线程扩展保持一致
- 在描述部分添加了说明 `asctime()` 函数不需要可重入的注释

**Issue 6**：
- `asctime_r()` 函数被标记为线程安全函数选项的一部分
- 标记了超出 ISO C 标准的扩展
- 应用程序使用部分更新为包含关于线程安全函数及其避免可能使用静态数据区域的说明
- 更新了 `asctime_r()` 的描述，描述返回字符串的格式
- 为与 ISO/IEC 9899:1999 标准保持一致，在 `asctime_r()` 原型中添加了 `restrict` 关键字
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/17，在返回值部分添加了 CX 扩展，要求如果 `asctime()` 函数不成功则返回 NULL

**Issue 7**：
- 应用了 Austin Group Interpretation 1003.1-2001 #053，将这些函数标记为过时
- 应用了 Austin Group Interpretation 1003.1-2001 #156
- `asctime_r()` 函数从线程安全函数选项移至基础标准
- 应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0033 [86,429]

**Issue 8**：
- 应用了 Austin Group Defect 469，阐明了 `asctime()` 行为未定义的条件
- 应用了 Austin Group Defect 1302，使此函数与 ISO/IEC 9899:2018 标准保持一致
- 应用了 Austin Group Defect 1330，更改了未来方向部分
- 应用了 Austin Group Defect 1376，从源自 ISO C 标准的某些文本中移除 CX 着色并更新以匹配 ISO C 标准
- **应用了 Austin Group Defect 1410，移除了 `asctime_r()` 函数**

---
