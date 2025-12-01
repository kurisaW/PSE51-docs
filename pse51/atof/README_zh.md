# atof

[ CX ] 本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意为之。本 POSIX.1-2024 卷遵循 ISO C 标准。

## 概要

```c
#include <stdlib.h>

double atof(const char *str);
```

## 描述

调用 `atof(str)` 应等效于：

```c
strtod(str, (char **)NULL),
```

除了错误处理可能有所不同。如果值无法表示，则行为未定义。

## 返回值

`atof()` 函数返回转换后的值。

## 错误

未定义任何错误。

## 示例

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    const char *str = "123.456";
    double result = atof(str);
    printf("Converted value: %f\n", result);
    return 0;
}
```

## 应用程序使用

`atof()` 函数不要求是线程安全的。

## 基本原理

以下章节为参考性内容。

*参考性文本结束。*