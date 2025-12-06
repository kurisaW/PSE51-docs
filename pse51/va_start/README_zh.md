# va_start, va_arg, va_copy, va_end - 处理可变参数列表

## 概要

```c
#include <stdarg.h>

type va_arg(va_list ap, type);
void va_copy(va_list dest, va_list src);
void va_end(va_list ap);
void va_start(va_list ap, argN);
```

## 描述

`<stdarg.h>` 头文件应包含一组宏，允许编写接受可变参数列表的可移植函数。具有可变参数列表的函数（如 `printf()`）如果不使用这些宏，则本质上是非可移植的，因为不同系统使用不同的参数传递约定。

`<stdarg.h>` 头文件应为用于遍历列表的变量定义 **va_list** 类型。

### va_start()

在调用任何 `va_arg()` 之前，应调用 `va_start()` 宏将 `ap` 初始化为列表的开始位置。

参数 `argN` 是函数定义中可变参数列表中最右边参数的标识符（即 `...` 之后的那一个）。如果参数 `argN` 使用 **register** 存储类别声明、具有函数类型或数组类型，或者具有与默认参数提升后所得类型不兼容的类型，则行为是未定义的。

### va_copy()

`va_copy()` 宏将 `dest` 初始化为 `src` 的副本，就像对 `dest` 应用了 `va_start()` 宏，然后使用与之前用来达到 `src` 当前状态相同序列的 `va_arg()` 宏一样。在没有对相同的 `dest` 进行中间调用 `va_end()` 宏的情况下，不应调用 `va_copy()` 或 `va_start()` 宏来重新初始化 `dest`。

### va_arg()

`va_arg()` 宏应返回 `ap` 所指向列表中的下一个参数。每次调用 `va_arg()` 都会修改 `ap`，以便依次返回连续参数的值。`type` 参数应是一个类型名称，其指定方式使得通过在 type 后后缀 '*' 可以简单地获得指向具有指定类型对象的指针类型。

如果没有实际的下一个参数，或者 `type` 与实际下一个参数的类型（根据默认参数提升进行提升后）不兼容，则行为是未定义的，但以下情况除外：

- 一种类型是有符号整数类型，另一种类型是相应的无符号整数类型，且值在两种类型中都可表示。
- 一种类型是指向 **void** 的指针，另一种类型是指向字符类型的指针。
- [XSI] 两种类型都是指针。

可以混合使用不同的类型，但由例程来知道期望的参数类型。

### va_end()

`va_end()` 宏用于清理；它使 `ap` 无效（除非再次调用 `va_start()` 或 `va_copy()`）。

## 用法

在同一个函数中，每次调用 `va_start()` 和 `va_copy()` 宏都应与对应的 `va_end()` 宏调用相匹配。

可以进行多次遍历，每次都由 `va_start()` ... `va_end()` 括起来。

对象 `ap` 可以作为参数传递给另一个函数；如果该函数使用参数 `ap` 调用 `va_arg()` 宏，则调用函数中 `ap` 的值是未指定的，并且在对 `ap` 的任何进一步引用之前应将其传递给 `va_end()` 宏。

## 示例

这是 `execl()` 的一个可能实现：

```c
#include <stdarg.h>

#define MAXARGS 31

/*
 * execl 的调用方式为
 * execl(file, arg1, arg2, ..., (char *)(0));
 */
int execl(const char *file, const char *args, ...)
{
    va_list ap;
    char *array[MAXARGS + 1];
    int argno = 0;

    va_start(ap, args);
    while (args != 0 && argno < MAXARGS)
    {
        array[argno++] = args;
        args = va_arg(ap, const char *);
    }
    array[argno] = (char *) 0;
    va_end(ap);
    return execv(file, array);
}
```

## 参见

- [`printf()`](printf.html)
- [`execl()`](execl.html)
- [`execv()`](execv.html)

---
