# unsetenv

## SYNOPSIS

```c
#include <stdlib.h>

int unsetenv(const char *name);
```

## DESCRIPTION

`unsetenv()` 函数应从调用进程的环境中删除一个环境变量。`name` 参数指向一个字符串，该字符串是要删除的变量的名称。命名参数不得包含 '=' 字符。如果指定的变量在当前环境中不存在，环境应保持不变，该函数应被视为已成功完成。

`unsetenv()` 函数应更新 `environ` 指向的指针列表。

`unsetenv()` 函数不需要是线程安全的。

## RETURN VALUE

成功完成后，应返回零。否则，应返回 -1，设置 `errno` 以指示错误，且环境应保持不变。

## ERRORS

`unsetenv()` 函数应在以下情况下失败：

- **[EINVAL]**
  - `name` 参数指向空字符串，或指向包含 '=' 字符的字符串。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

请参考 `setenv()` 中的 RATIONALE 部分。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `getenv()`
- `setenv()`
- XBD `<stdlib.h>`, `<sys/types.h>`

## CHANGE HISTORY

首次发布于 Issue 6。源于 IEEE P1003.1a 草案标准。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #156。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0698 [167] 和 XSH/TC1-2008/0699 [185]。

---

*UNIX® 是 The Open Group 的注册商标。*
*POSIX™ 是 The IEEE 的商标。*
*Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*