# fdatasync

## SYNOPSIS (概要)

```c
#include <unistd.h>

int fdatasync(int fildes);
```

## DESCRIPTION (描述)

`fdatasync()` 函数应强制将与文件描述符 `fildes` 指示的文件相关联的所有当前排队 I/O 操作推进到同步 I/O 完成状态。

该功能应等价于定义了符号 _POSIX_SYNCHRONIZED_IO 的 `fsync()`，但区别在于所有 I/O 操作应按照同步 I/O 数据完整性完成的规定来完成。

## RETURN VALUE (返回值)

如果成功，`fdatasync()` 函数应返回值 0；否则，函数应返回值 -1 并设置 `errno` 来指示错误。如果 `fdatasync()` 函数失败，不保证未完成的 I/O 操作已经完成。

## ERRORS (错误)

`fdatasync()` 函数应在以下情况失败：

- **[EBADF]**
  `fildes` 参数不是有效的文件描述符。

- **[EINVAL]**
  此实现不支持对此文件的同步 I/O。

如果任何排队 I/O 操作失败，`fdatasync()` 应返回为 `read()` 和 `write()` 定义的错误条件。

---

*以下部分为参考信息。*

## EXAMPLES (示例)

无。

## APPLICATION USAGE (应用程序使用)

注意，即使文件描述符不是以写入方式打开的，如果基础文件上有任何待处理的写入请求，那么在 `fdatasync()` 返回之前将完成这些 I/O 操作。

修改目录的应用程序（例如，通过在目录中创建文件）可以对该目录��用 `fdatasync()` 以确保目录条目被同步，但对于大多数应用程序来说，这应该是不必要的（参见 XBD [4.11 文件系统缓存](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_11)）。

## RATIONALE (基本原理)

无。

## FUTURE DIRECTIONS (未来方向)

无。

## SEE ALSO (参见)

- [`aio_fsync()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_fsync.html)
- [`fcntl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fcntl.html)
- [`fsync()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsync.html)
- [`open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/open.html)
- [`read()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html)
- [`write()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/write.html)
- XBD [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## CHANGE HISTORY (变更历史)

**首次发布于第 5 版。** 为了与 POSIX 实时扩展对齐而包含。

### Issue 6

[ENOSYS] 错误条件已被删除，因为如果实现不支持同步输入和输出选项，则无需提供存根。

`fdatasync()` 函数被标记为同步输入和输出选项的一部分。

### Issue 7

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0110 [501]。

### Issue 8

应用了 Austin Group Defect 672，更改了应用程序使用部分。

*参考文本结束。*

---
