# time — 获取时间

## SYNOPSIS

```c
#include <time.h>

time_t time(time_t *tloc);
```

## DESCRIPTION

[CX] 本参考页上描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本卷遵循 ISO C 标准。

`time()` 函数应返回自纪元（Epoch）以来经过的秒数时间值 [CX]。

`tloc` 参数指向一个区域，该区域也存储返回值。如果 `tloc` 是空指针，则不存储任何值。

## RETURN VALUE

成功完成后，`time()` 应返回时间值。否则，应返回 `(time_t)-1`。

## ERRORS

`time()` 函数可能在以下情况下失败：

- **[EOVERFLOW]** [CX] 自纪元以来的秒数无法放入 `time_t` 类型的对象中。

*以下章节为信息性内容。*

## EXAMPLES

### 获取当前时间

以下示例使用 `time()` 函数计算自纪元以来经过的秒数，使用 `localtime()` 将该值转换为分解时间，并使用 `asctime()` 将分解时间值转换为可打印字符串。

```c
#include <stdio.h>
#include <time.h>

int main(void)
{
    time_t result;

    result = time(NULL);
    printf("%s%ju secs since the Epoch\n",
        asctime(localtime(&result)),
            (uintmax_t)result);
    return(0);
}
```

此示例以类似以下格式将当前时间写入 `stdout`：

```
Wed Jun 26 10:32:15 1996
835810335 secs since the Epoch
```

### 为事件计时

以下示例获取当前时间，以用户格式输出，并打印到正在计时的事件的分钟数。

```c
#include <time.h>
#include <stdio.h>
...
time_t now;
int minutes_to_event;
...
time(&now);
minutes_to_event = ...;
printf("The time is ");
puts(asctime(localtime(&now)));
printf("There are %d minutes to the event.\n",
    minutes_to_event);
...
```

## APPLICATION USAGE

无。

## RATIONALE

`time()` 函数以秒为单位返回值，而 `clock_gettime()` 返回 `struct timespec`（秒和纳秒），因此能够返回更精确的时间。`times()` 函数也比 `time()` 能够提供更高的精度，因为它以时钟滴答为单位返回值，尽管它返回的是从任意点（如系统启动时间）开始经过的时间，而不是从纪元开始。

本标准的早期版本允许 `time_t` 的宽度小于 64 位。32 位有符号整数（在许多历史实现中使用）在 2038 年会失效，虽然 32 位无符号整数要到 2106 年才会失效，但首选的解决方案是使 `time_t` 更宽，而不是使其变为无符号类型。

在某些系统上，`time()` 函数使用系统调用实现，该系统调用除了返回值之外不返回错误条件。在这些系统上，无法区分有效和无效的返回值，因此无法可靠地检测溢出条件。

使用 `<time.h>` 头文件而不是 `<sys/types.h>` 允许与 ISO C 标准兼容。

许多历史实现（包括第 7 版）和 1984 年的 /usr/group 标准使用 `long` 而不是 `time_t`。为了与 ISO C 标准保持一致，POSIX.1-2024 本卷使用后一种类型。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `asctime()`
- `clock()`
- `clock_getres()`
- `ctime()`
- `difftime()`
- `futimens()`
- `gmtime()`
- `localtime()`
- `mktime()`
- `strftime()`
- `strptime()`
- `times()`

XBD `<time.h>`

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

添加了 EXAMPLES、RATIONALE 和 FUTURE DIRECTIONS 章节。

### Issue 7

应用了 POSIX.1-2008，Technical Corrigendum 1，XSH/TC1-2008/0663 [106]、XSH/TC1-2008/0664 [350]、XSH/TC1-2008/0665 [106]、XSH/TC1-2008/0666 [350] 和 XSH/TC1-2008/0667 [350]。

### Issue 8

应用了 Austin Group Defect 1330，移除了过时的接口。

应用了 Austin Group Defect 1462，更改了 RATIONALE 和 FUTURE DIRECTIONS 章节。

*信息性文本结束。*

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 IEEE 的商标。*
*版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利*