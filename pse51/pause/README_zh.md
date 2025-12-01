# pause

Open Group 基础规范第8版
IEEE Std 1003.1-2024
版权所有 © 2001-2024 IEEE 和 The Open Group

## 名称

**pause** — 暂停线程直到接收到信号

## 概要

```c
#include <unistd.h>

int pause(void);
```

## 描述

`pause()` 函数应暂停调用线程，直到传递一个其动作是执行信号捕获函数或终止进程的信号。

如果动作是终止进程，`pause()` 不应返回。

如果动作是执行信号捕获函数，`pause()` 应在信号捕获函数返回后返回。

## 返回值

由于 `pause()` 无限期暂停线程执行，除非被信号中断，因此没有成功完成的返回值。应返回值 -1 并设置 `errno` 以指示错误。

## 错误

`pause()` 函数在以下情况下应失败：

- **[EINTR]**
  调用进程捕获了一个���号，并且控制从信号捕获函数返回。

*以下各节为参考信息。*

## 示例

无。

## 应用程序使用

`pause()` 的许多常见用法存在时间窗口问题。这种情况涉及检查与信号相关的条件，如果信号尚未发生，则调用 `pause()`。当信号在检查和调用 `pause()` 之间发生时，进程通常会无限期阻塞。可以使用 `sigprocmask()` 和 `sigsuspend()` 函数来避免这类问题。

## 基本原理

无。

## 未来方向

无。

## 参见

- [`sigsuspend()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sigsuspend.html)
- XBD [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## 变更历史

首次发布于第1版。派生自 SVID 第1版。

### 第5版

为与 POSIX 线程扩展对齐，更新了描述部分。

### 第6版

添加了应用程序使用部分。

*参考信息结束。*

---

UNIX® 是 The Open Group 的注册商标。
POSIX™ 是 IEEE 的商标。
版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利
[ 主索引 | XBD | XSH | XCU | XRAT ]