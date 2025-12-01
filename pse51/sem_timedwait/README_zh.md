# sem_clockwait, sem_timedwait — 锁定信号量

## 概要

```c
#include <semaphore.h>

int sem_clockwait(sem_t *restrict sem,
                 clockid_t clock_id,
                 const struct timespec *restrict abstime);

int sem_timedwait(sem_t *restrict sem,
                  const struct timespec *restrict abstime);
```

## 描述

`sem_clockwait()` 和 `sem_timedwait()` 函数应锁定 `sem` 引用的信号量，如同 `sem_wait()` 函数一样。但是，如果信号量无法在不等待另一个进程或线程通过执行 `sem_post()` 函数来解锁信号量的情况下被锁定，则当指定的超时时间到期时，此等待应被终止。

当 `abstime` 指定的绝对时间经过时，超时应到期，这是基于超时的时钟测量的（即，当时钟值等于或超过 `abstime` 时），或者如果在调用时 `abstime` 指定的绝对时间已经过去。

对于 `sem_timedwait()`，超时应基于 CLOCK_REALTIME 时钟。对于 `sem_clockwait()`，超时应基于 `clock_id` 参数指定的时钟。超时的分辨率应是其所基于时钟的分辨率。实现应支持将 CLOCK_REALTIME 和 CLOCK_MONOTONIC 作为 `clock_id` 参数传递给 `sem_clockwait()`。

在任何情况下，如果信号量可以立即锁定，函数不应因超时而失败。如果信号量可以立即锁定，则无需检查 `abstime` 的有效性。

## 返回值

如果调用进程成功在指定信号量 `sem` 上执行了信号量锁定操作，`sem_clockwait()` 和 `sem_timedwait()` 函数应返回零。如果调用不成功，信号量的状态应保持不变，函数应返回值 -1 并设置 `errno` 来指示错误。

## 错误

`sem_clockwait()` 和 `sem_timedwait()` 函数在以下情况下应失败：

- **[EINVAL]**
  - 进程或线程将会阻塞，且 `abstime` 参数指定的纳秒字段值小于零或大于等于 1000 百万，或者 `sem_clockwait()` 函数传递了无效或不支持的 `clock_id` 值。

- **[ETIMEDOUT]**
  - 在指定的超时时间到期之前无法锁定信号量。

`sem_clockwait()` 和 `sem_timedwait()` 函数在以下情况下可能失败：

- **[EDEADLK]**
  - 检测到死锁条件。

- **[EINTR]**
  - 信号中断了函数。

- **[EINVAL]**
  - `sem` 参数不引用有效的信号量。

## 示例

下面显示的程序操作一个未命名信号量。程序期望两个命令行参数。第一个参数指定一个秒值，用于设置闹钟定时器来生成 SIGALRM 信号。此处理程序执行 `sem_post()` 来增加在 `main()` 中使用 `sem_clockwait()` 等待的信号量。第二个命令行参数指定 `sem_clockwait()` 的超时长度，以秒为单位。

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <semaphore.h>
#include <time.h>
#include <assert.h>
#include <errno.h>
#include <signal.h>

sem_t sem;

static void
handler(int sig)
{
    int sav_errno = errno;
    static const char info_msg[] = "sem_post() from handler\n";
    write(STDOUT_FILENO, info_msg, sizeof info_msg - 1);
    if (sem_post(&sem) == -1) {
        static const char err_msg[] = "sem_post() failed\n";
        write(STDERR_FILENO, err_msg, sizeof err_msg - 1);
        _exit(EXIT_FAILURE);
    }
    errno = sav_errno;
}

int
main(int argc, char *argv[])
{
    struct sigaction sa;
    struct timespec ts;
    int s;

    if (argc != 3) {
        fprintf(stderr, "Usage: %s <alarm-secs> <wait-secs>\n",
            argv[0]);
        exit(EXIT_FAILURE);
    }

    if (sem_init(&sem, 0, 0) == -1) {
        perror("sem_init");
        exit(EXIT_FAILURE);
    }

    /* 建立 SIGALRM 处理程序；使用 argv[1] 设置闹钟定时器 */

    sa.sa_handler = handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;
    if (sigaction(SIGALRM, &sa, NULL) == -1) {
        perror("sigaction");
        exit(EXIT_FAILURE);
    }

    alarm(atoi(argv[1]));

    /* 计算相对间隔为当前时间加上 argv[2] 给定的秒数 */

    if (clock_gettime(CLOCK_MONOTONIC, &ts) == -1) {
        perror("clock_gettime");
        exit(EXIT_FAILURE);
    }
    ts.tv_sec += atoi(argv[2]);

    printf("main() about to call sem_clockwait()\n");
    while ((s = sem_clockwait(&sem, CLOCK_MONOTONIC, &ts)) == -1 &&
           errno == EINTR)
        continue;       /* 如果被处理程序中断则重启 */

    /* 检查发生了什么 */

    if (s == -1) {
        if (errno == ETIMEDOUT)
            printf("sem_clockwait() timed out\n");
        else
            perror("sem_clockwait");
    } else
        printf("sem_clockwait() succeeded\n");

    exit((s == 0) ? EXIT_SUCCESS : EXIT_FAILURE);
}
```

## 应用程序用法

使用这些函数的应用程序可能会受到优先级反转的影响，如 XBD 3.275 优先级反转中所述。

## 原理

无。

## 未来方向

无。

## 另请参阅

- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)
- [`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html)
- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`time()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/time.html)

XBD 3.275 优先级反转, `<semaphore.h>`, `<time.h>`

## 变更历史

### 首次发布于 Issue 6。源自 IEEE Std 1003.1d-1999。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/120，更新了 ERRORS 部分，使得 [EINVAL] 错误变为可选的。

### Issue 7

`sem_timedwait()` 函数从信号量选项移至基础部分。

与定时器选项相关的功能移至基础部分。

添加了一个示例。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0529 [138]。

### Issue 8

应用了 Austin Group Defect 592，从 SYNOPSIS 和 DESCRIPTION 部分移除了与 `<time.h>` 相关的文本。

应用了 Austin Group Defect 1216，添加了 `sem_clockwait()`。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*版权所有 © 2001-2024 IEEE 和 The Open Group*