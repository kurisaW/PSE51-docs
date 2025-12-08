# setenv — 添加或更改环境变量

## 概要 (SYNOPSIS)

```c
#include <stdlib.h>

int setenv(const char *envname, const char *envval, int overwrite);
```

## 描述 (DESCRIPTION)

`setenv()` 函数应在调用进程的环境中更新或添加变量。`envname` 参数指向一个字符串，该字符串包含要添加或修改的环境变量的名称。环境变量应设置为 `envval` 所指向的值。如果 `envname` 指向的字符串包含 '=' 字符，则函数应失败。

如果 `envname` 指定的环境变量已经存在且 `overwrite` 的值非零，则函数应返回成功并更新环境。如果 `envname` 指定的环境变量已经存在且 `overwrite` 的值为零，则函数应返回成功且环境保持不变。

`setenv()` 函数应更新 `environ` 所指向的指针列表。

`envname` 和 `envval` 描述的字符串会被此函数复制。

`setenv()` 函数不必是线程安全的。

## 返回值 (RETURN VALUE)

成功完成后，应返回零。否则，应返回 -1，并设置 `errno` 以指示错误，且环境应保持不变。

## 错误 (ERRORS)

`setenv()` 函数应在以下情况下失败：

- **[EINVAL]**
  - `envname` 参数指向空字符串或指向包含 '=' 字符的字符串。

- **[ENOMEM]**
  - 没有足够的可用内存来将变量或其值添加到环境中。

## 示例 (EXAMPLES)

无。

## 应用程序使用 (APPLICATION USAGE)

关于在多线程应用程序中更改环境的限制，请参见 `exec()`。

## 基本原理 (RATIONALE)

如果 `setenv()` 更改外部变量 `environ`，可能会发生预期之外的结果。特别是，如果 `main()` 的可选 `envp` 参数存在，它不会被更改，因此可能指向环境的过时副本（`environ` 的任何其他副本也是如此）。然而，除了上述限制之外，标准开发者希望支持通过 `environ` 指针遍历环境的传统方法。

标准开发者决定在此版本中要求 `setenv()`，因为它解决了一个缺失的功能，并且不会对实现者造成重大负担。

关于是否应该将 System V 的 `putenv()` 函数或 BSD 的 `setenv()` 函数作为必需函数存在相当大的争议。最终选择了 `setenv()` 函数，因为它允许实现 `unsetenv()` 函数来删除环境变量，而无需指定额外的接口。`putenv()` 函数作为 XSI 选项的一部分可用。

标准开发者曾考虑要求当调用 `setenv()` 会导致超过 {ARG_MAX} 时指示错误。该要求被拒绝，因为这种条件可能是暂时的，应用程序最终可能会减少环境大小。最终的成功或失败取决于调用 `exec` 时的大小，`exec()` 会返回此错误条件的指示。

另参见 `getenv()` 的基本原理部分。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 另见 (SEE ALSO)

[exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html),
[getenv()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getenv.html),
[putenv()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/putenv.html),
[unsetenv()](https://pubs.opengroup.org/onlinepubs/9799919799/functions/unsetenv.html)

XBD [<stdlib.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdlib.h.html),
[<sys/types.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html),
[<unistd.h>](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## 变更历史 (CHANGE HISTORY)

首次发布于 Issue 6。源自 IEEE P1003.1a 草案标准。

IEEE Std 1003.1-2001/Cor 1-2002，项目 XSH/TC1/D6/55 被应用，在应用程序使用和另见部分添加了对 `exec` 的引用。

### Issue 7

Austin Group Interpretation 1003.1-2001 #156 被应用。

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0549 [167], XSH/TC1-2008/0550 [185], XSH/TC1-2008/0551 [167], 和 XSH/TC1-2008/0552 [38] 被应用。

---
