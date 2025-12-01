# free — 释放已分配的内存

## 概要

```c
#include <stdlib.h>

void free(void *ptr);
```

## 描述

`free()` 函数应导致由 `ptr` 指向的空间被释放；即，使其可用于进一步的分配。如果 `ptr` 是空指针，则不应发生任何操作。否则，如果参数不匹配先前由 `aligned_alloc()`、`calloc()`、`malloc()`、`posix_memalign()`、`realloc()`、`reallocarray()` 或 POSIX.1-2024 中如同 `malloc()` 一样分配内存的函数返回的指针，或者如果该空间已通过 `free()`、`reallocarray()` 或 `realloc()` 调用释放，则行为是未定义的。

对指向已释放空间的指针的任何使用都会导致未定义行为。

如果 `ptr` 是空指针或先前如同由 `malloc()` 返回且尚未释放的指针，`free()` 函数不应修改 `errno`。

为了确定数据竞争的存在性，`free()` 应表现得好像它只访问通过其参数可访问的内存位置，而不访问其他静态持续时间存储。但是，该函数可以可见地修改其释放的存储。分配或释放特定内存区域的 `aligned_alloc()`、`calloc()`、`free()`、`malloc()`、`posix_memalign()`、`reallocarray()` 和 `realloc()` 调用应发生在单一的总顺序中，并且每个此类释放调用应与此顺序中的下一个分配（如果有）同步。

## 返回值

`free()` 函数不应返回值。

## 错误

未定义任何错误。

## 示例

无。

## 应用程序使用

现在不再要求实现支持包含 `<malloc.h>`。

因为 `free()` 函数对于有效指针不会修改 `errno`，所以可以安全地在清理代码中使用它而不会破坏之前的错误，如以下示例代码：

```c
// buf 是通过 malloc(buflen) 获得的
ret = write(fd, buf, buflen);
if (ret < 0) {
    free(buf);
    return ret;
}
```

但是，本标准的早期版本没有要求这一点，相同的示例必须写为：

```c
// buf 是通过 malloc(buflen) 获得的
ret = write(fd, buf, buflen);
if (ret < 0) {
    int save = errno;
    free(buf);
    errno = save;
    return ret;
}
```

## 原理

无。

## 未来方向

无。

## 另请参阅

- `aligned_alloc()`
- `calloc()`
- `malloc()`
- `posix_memalign()`
- `realloc()`
- `<stdlib.h>`

## 变更历史

### 首次发布于 Issue 1
源自 SVID 的 Issue 1。

### Issue 6
删除了对 `valloc()` 函数的引用。

### Issue 7
更新了 DESCRIPTION，以阐明如果返回的指针不是由如同 `malloc()` 一样分配内存的函数返回的，则行为是未定义的。

### Issue 8
应用了 Austin Group Defect 385，增加了 `free()` 在传递给可释放对象的指针时不修改 `errno` 的要求。

应用了 Austin Group Defect 1218，增加了 `reallocarray()`。

应用了 Austin Group Defect 1302，使此函数与 ISO/IEC 9899:2018 标准保持一致。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*