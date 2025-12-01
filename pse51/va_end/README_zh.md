# va_end

## NAME

va_end - 处理可变参数列表

## SYNOPSIS

```c
#include <stdarg.h>

void va_end(va_list ap);
```

## DESCRIPTION

`va_end()` 宏用于在遍历可变参数列表后进行清理。它使 `va_list` 对象 `ap` 失效（除非再次调用 `va_start()` 或 `va_copy()`）。

本参考页面描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意为之。POSIX.1-2024 卷遵从 ISO C 标准。

## REQUIREMENTS

对 `va_start()` 和 `va_copy()` 宏的每次调用都必须在同一个函数中有对应的 `va_end()` 宏调用。

## USAGE RULES

- 对象 `ap` 可以作为参数传递给另一个函数
- 如果该函数使用参数 `ap` 调用 `va_arg()` 宏，则在调用函数中 `ap` 的值是未指定的，在对 `ap` 进行任何进一步引用之前，应将其传递给 `va_end()` 宏
- 在没有对同一 `ap` 进行中间 `va_end()` 宏调用的情况下，不得调用 `va_copy()` 或 `va_start()` 宏来重新初始化 `ap`

## EXAMPLE

```c
#include <stdarg.h>
#include <unistd.h>
#include <stdio.h>

#define MAXARGS 31

/*
 * execl 通过以下方式调用
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
        array[argno++] = args;  // 修正了原代码中的语法错误
        args = va_arg(ap, const char *);
    }
    array[argno] = (char *) 0;
    va_end(ap);
    return execv(file, array);
}
```

## RELATED FUNCTIONS

- `va_start()` - 初始化可变参数列表
- `va_arg()` - 从可变参数列表中检索下一个参数
- `va_copy()` - 复制可变参数列表状态

## SEE ALSO

- [`printf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/printf.html)
- [`execl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/execl.html)

---

*Source: The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*