# nanosleep — 高精度睡眠

## SYNOPSIS

```c
#include <time.h>

int nanosleep(const struct timespec *rqtp, struct timespec *rmtp);
```

## DESCRIPTION

`nanosleep()` 函数应导致当前线程暂停执行，直到由 `rqtp` 参数指定的时间间隔已经过去，或者向调用线程发送了一个信号，且该信号的动作是调用信号捕获函数或终止进程。暂停时间可能比请求的时间更长，因为参数值会向上舍入为睡眠分辨率的整数倍，或者因为系统对其他活动的调度。但是，除了被信号中断的情况外，暂停时间不应小于由 `rqtp` 指定的时间，这是由系统时钟 CLOCK_REALTIME 测量的。

使用 `nanosleep()` 函数对任何信号的动作或阻塞没有影响。

## RETURN VALUE

如果 `nanosleep()` 函数因为请求的时间已过去而返回，其返回值应为零。

如果 `nanosleep()` 函数因为被信号中断而返回，它应返回值 -1 并设置 `errno` 来指示中断。如果 `rmtp` 参数非 NULL，它所引用的 `timespec` 结构会被更新以包含时间间隔中剩余的时间量（请求时间减去实际睡眠的时间）。`rqtp` 和 `rmtp` 参数可以指向同一个对象。如果 `rmtp` 参数为 NULL，则不返回剩余时间。

如果 `nanosleep()` 失败，它应返回值 -1 并设置 `errno` 来指示错误。

## ERRORS

`nanosleep()` 函数在以下情况应失败：

- **[EINTR]**
  `nanosleep()` 函数被信号中断。

- **[EINVAL]**
  `rqtp` 参数指定的纳秒值小于零或大于等于 1000 百万。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

为了轮询非中断函数的状态，常常需要暂停线程的执行。大量实际需求可以通过对 `sleep()` 的简单扩展来满足，该扩展提供更精细的分辨率。

在 POSIX.1-1990 标准和 SVR4 中，可以实现这样的例程，但唤醒的频率受到 `alarm()` 和 `sleep()` 函数分辨率的限制。在 4.3 BSD 中，可以使用无静态存储且不保留系统设施的方式编写这样的例程。虽然可以使用剩余的 `timer_*()` 函数编写与 `sleep()` 功能相似的函数，但这样的函数需要使用信号并保留一些信号编号。POSIX.1-2024 要求 `nanosleep()` 不干扰信号功能。

`nanosleep()` 函数在成功时应返回值 0，在失败或被中断时应返回 -1。后一种情况与 `sleep()` 不同。这样做是因为剩余时间通过参数结构指针 `rmtp` 返回，而不是作为返回值返回。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `clock_nanosleep()`
- `sleep()`
- `<time.h>`

## CHANGE HISTORY

### 首次发布于 Issue 5
为与 POSIX 实时扩展对齐而包含。

### Issue 6
- `nanosleep()` 函数被标记为定时器选项的一部分。
- 移除了 [ENOSYS] 错误条件，因为如果实现不支持定时器选项，则无需提供存根。
- 应用了 IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/37，更新 SEE ALSO 部分以包含 `clock_nanosleep()` 函数。
- 应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/63，更正了 RATIONALE 部分的文本。

### Issue 7
- 应用了 SD5-XBD-ERN-33。
- `nanosleep()` 函数从定时器选项移至基础。
- 应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0231 [909]。

---
