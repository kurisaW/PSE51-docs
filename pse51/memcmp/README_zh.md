# memcmp — 内存字节比较

## SYNOPSIS

```c
#include <string.h>

int memcmp(const void *s1, const void *s2, size_t n);
```

## DESCRIPTION

`memcmp()` 函数应将 `s1` 所指向对象的前 `n` 个字节（每个字节都被解释为 `unsigned char` 类型）与 `s2` 所指向对象的前 `n` 个字节进行比较。

非零返回值的符号应由所比较对象中第一对不同的字节（都被解释为 `unsigned char` 类型）的值之间的差的符号确定。

`memcmp()` 函数在有效输入上不应改变 `errno` 的设置。

## RETURN VALUE

如果 `s1` 所指向的对象大于、等于或小于 `s2` 所指向的对象，`memcmp()` 函数应分别返回大于、等于或小于 0 的整数。

## ERRORS

未定义错误。

## EXAMPLES

无。

## APPLICATION USAGE

无。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `<string.h>`

## CHANGE HISTORY

首次发布于 Issue 1。源自 SVID 的 Issue 1。

### Issue 8

应用了 Austin Group Defect 448，增加了 `memcmp()` 在有效输入上不改变 `errno` 设置的要求。

---
