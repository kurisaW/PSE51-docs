# fegetround, fesetround — 获取和设置当前舍入方向

## 概要

```c
#include <fenv.h>

int fegetround(void);
int fesetround(int round);
```

## 描述

[CX] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是非故意的。POSIX.1-2024 本文遵循 ISO C 标准。

`fegetround()` 函数应获取当前的舍入方向。

`fesetround()` 函数应建立由其参数 `round` 表示的舍入方向。如果参数不等于舍入方向宏的值，则舍入方向不会改变。

## 返回值

`fegetround()` 函数应返回表示当前舍入方向的舍入方向宏的值，或者当没有相应的舍入方向宏或当前舍入方向无法确定时返回负值。

`fesetround()` 函数当且仅当所请求的舍入方向已建立时返回零值。

## 错误

未定义任何错误。

---

*以下章节为补充信息。*

## 示例

以下示例保存、设置和恢复舍入方向，如果设置舍入方向失败则报告错误并中止：

```c
#include <fenv.h>
#include <assert.h>

void f(int round_dir)
{
    #pragma STDC FENV_ACCESS ON
    int save_round;
    int setround_ok;

    save_round = fegetround();
    setround_ok = fesetround(round_dir);
    assert(setround_ok == 0);
    /* ... */
    fesetround(save_round);
    /* ... */
}
```

## 应用用法

无。

## 原理

无。

## 未来方向

无。

## 参见

XBD [`<fenv.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fenv.h.html)

## 变更历史

首次发布于 Issue 6。源自 ISO/IEC 9899:1999 标准。

ISO/IEC 9899:1999 标准，技术勘误 1 已被纳入。

*补充信息结束。*

---
