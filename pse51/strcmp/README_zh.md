# strcmp — 比较两个字符串

## 概要 (SYNOPSIS)

```c
#include <string.h>

int strcmp(const char *s1, const char *s2);
```

## 描述 (DESCRIPTION)

本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。本卷 POSIX.1-2024 遵从 ISO C 标准。

`strcmp()` 函数应将 `s1` 指向的字符串与 `s2` 指向的字符串进行比较。

非零返回值的符号应由被比较字符串中第一对相异字节（两者均解释为 `unsigned char` 类型）的值的差值符号确定。

`strcmp()` 函数在有效输入时不应改变 `errno` 的设置。

## 返回值 (RETURN VALUE)

完成时，如果 `s1` 指向的字符串分别大于、等于或小于 `s2` 指向的字符串，`strcmp()` 应返回一个大于、等于或小于 0 的整数。

## 错误 (ERRORS)

未定义错误。

---

## 示例 (EXAMPLES)

### 检查密码条目 (Checking a Password Entry)

以下示例将从标准输入读取的信息与用户条目的名称值进行比较。如果 `strcmp()` 函数返回 0（表示匹配），将进一步检查用户是否输入了正确的旧密码。`crypt()` 函数应使用 `passwd` 结构中加密密码的值作为盐值来加密用户输入的旧密码。如果此值与结构中加密的 `passwd` 的值匹配，则输入的密码 `oldpasswd` 是正确的用户密码。最后，程序加密新密码，以便将信息存储在 `passwd` 结构中。

```c
#include <string.h>
#include <unistd.h>
#include <stdio.h>
...
int valid_change;
struct passwd *p;
char user[100];
char oldpasswd[100];
char newpasswd[100];
char savepasswd[100];
...
if (strcmp(p->pw_name, user) == 0) {
    if (strcmp(p->pw_passwd, crypt(oldpasswd, p->pw_passwd)) == 0) {
        strcpy(savepasswd, crypt(newpasswd, user));
        p->pw_passwd = savepasswd;
        valid_change = 1;
    }
    else {
        fprintf(stderr, "Old password is not valid\n");
    }
}
...
```

## 应用用法 (APPLICATION USAGE)

无。

## 基本原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 另请参见 (SEE ALSO)

- [`strncmp()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/strncmp.html)
- [`<string.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/string.h.html)

## 变更历史 (CHANGE HISTORY)

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 6

标记了超出 ISO C 标准的扩展。

### Issue 8

应用了 Austin Group 缺陷 448，增加了 `strcmp()` 在有效输入时不改变 `errno` 设置的要求。

---
