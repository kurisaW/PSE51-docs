# uname

## 概要

```c
#include <sys/utsname.h>

int uname(struct utsname *name);
```

## 描述

`uname()` 函数应将标识当前系统的信息存储在 `name` 所指向的结构中。

`uname()` 函数使用在 `<sys/utsname.h>` 中定义的 **utsname** 结构。

`uname()` 函数应在字符数组 `sysname` 中返回命名当前系统的字符串。类似地，`nodename` 应包含此节点在实现定义的通信网络中的名称。数组 `release` 和 `version` 应进一步标识操作系统。数组 `machine` 应包含标识系统正在运行的硬件的名称。

每个成员的格式是实现定义的。

## 返回值

成功完成后，`uname()` 应返回 0；否则，应返回 -1 并设置 `errno` 以指示错误。

## 错误

未定义错误。

## 示例

### 示例 1：获取系统信息

```c
#include <stdio.h>
#include <sys/utsname.h>

int main(void)
{
    struct utsname name;

    if (uname(&name) == -1) {
        perror("uname");
        return 1;
    }

    printf("System name: %s\n", name.sysname);
    printf("Node name:   %s\n", name.nodename);
    printf("Release:     %s\n", name.release);
    printf("Version:     %s\n", name.version);
    printf("Machine:     %s\n", name.machine);

    return 0;
}
```

## 应用程序使用

结构成员的值不受约束，与操作系统中实现的 POSIX.1-2024 本版本没有任何关系。应用程序应依赖于在 `<unistd.h>` 中定义的 `_POSIX_VERSION` 和相关常量。

POSIX.1-2024 本版本未定义结构成员的大小，并允许它们具有不同的大小，尽管大多数实现将它们定义为相同的大小：八字节加上字符串终止符的一字节。`nodename` 的大小不足以用于许多网络。

## 基本原理

`uname()` 函数起源于 System III、System V 和相关实现，在 Version 7 或 4.3 BSD 中不存在。在那些历史实现中，它返回的值是在系统编译时设置的。

4.3 BSD 有 `gethostname()` 和 `gethostid()`，它们分别返回符号名称和数值。还有相关的 `sethostname()` 和 `sethostid()` 函数，用于设置其他两个函数返回的值。前者包含在此规范中，后者不包含。

## 未来方向

无。

## 另见

- [`gethostname()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostname.html)
- [`gethostid()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/gethostid.html)
- [`<sys/utsname.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_utsname.h.html)
- [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## 变更历史

* 首次发布于 Issue 1。派生自 System V 文档。

* 版权所有 © 2018-2024, The Open Group。保留所有权利。
 SPDX-License-Identifier: CC-BY-4.0