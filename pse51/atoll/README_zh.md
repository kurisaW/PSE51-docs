# atoll

## SYNOPSIS

```c
#include <stdlib.h>

long atol(const char *nptr);
long long atoll(const char *nptr);
```

## DESCRIPTION

[选项开始] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 卷遵循 ISO C 标准。[选项结束]

除下面说明的情况外，调用 `atol(nptr)` 应等效于：

```c
strtol(nptr, (char **)NULL, 10)
```

除下面说明的情况外，调用 `atoll(nptr)` 应等效于：

```c
strtoll(nptr, (char **)NULL, 10)
```

错误处理可能有所不同。如果值无法表示，则行为未定义。

## RETURN VALUE

这些函数返回转换后的值。

## ERRORS

未定义错误。

## EXAMPLES

未提供示例。

## APPLICATION USAGE

未提供应用程序用法。

## RATIONALE

未提供基本原理。

## FUTURE DIRECTIONS

未提供未来方向。

## SEE ALSO

`strtol()`, `strtoll()`

## CHANGE HISTORY

已应用 SD5-XSH-ERN-61，更正了 `atoll()` 的 DESCRIPTION。

已应用 POSIX.1-2008，Technical Corrigendum 2，XSH/TC2-2008/0046 [892]。

*信息性文本结束。*
