# shm_unlink — 删除共享内存对象 (REALTIME)

## 概要 (SYNOPSIS)

```c
#include <sys/mman.h>

int shm_unlink(const char *name);
```

## 描述 (DESCRIPTION)

`shm_unlink()` 函数应删除由 `name` 指针指向的字符串命名的共享内存对象的名称。

如果在解除链接（unlink）时共享内存对象存在一个或多个引用，则名称应在 `shm_unlink()` 返回之前被删除，但内存对象内容的删除应推迟到所有对该共享内存对象的打开和映射引用都被移除之后。

即使对象在最后一次 `shm_unlink()` 之后继续存在，后续重用该名称应导致 `shm_open()` 表现得好像不存在此名称的共享内存对象（即，如果未设置 O_CREAT，`shm_open()` 应失败，或者如果设置了 O_CREAT，应创建一��新的共享内存对象）。

## 返回值 (RETURN VALUE)

成功完成时，应返回零值。否则，应返回 -1 并设置 `errno` 以指示错误。如果返回 -1，则命名的共享内存对象不应被此函数调用修改。

## 错误 (ERRORS)

`shm_unlink()` 函数在以下情况下应失败：

- **[EACCES]** - 拒绝解除链接命名共享内存对象的权限。
- **[ENOENT]** - 命名的共享内存对象不存在。

`shm_unlink()` 函数在以下情况下可能失败：

- **[ENAMETOOLONG]** - `name` 参数的长度在不支持 XSI 选项的系统上超过 {_POSIX_PATH_MAX} 或在 XSI 系统上超过 {_XOPEN_PATH_MAX}，或者包含的路径名组件在不支持 XSI 选项的系统上长于 {_POSIX_NAME_MAX} 或在 XSI 系统上长于 {_XOPEN_NAME_MAX}。使用包含与之前在成功的 `shm_open()` 调用中使用的相同共享内存对象名称的 `name` 参数调用 `shm_unlink()` 不应给出 [ENAMETOOLONG] 错误。

## 示例 (EXAMPLES)

无。

## 应用程序用法 (APPLICATION USAGE)

使用 `open()` 分配的内存对象名称以通常方式用 `unlink()` 删除。使用 `shm_open()` 分配的内存对象名称用 `shm_unlink()` 删除。请注意，如果内存对象已经在使用中，实际的内存对象在最后一次关闭和取消映射之前不会被销毁。

## 基本原理 (RATIONALE)

无。

## 未来方向 (FUTURE DIRECTIONS)

未来版本可能要求 `shm_open()` 和 `shm_unlink()` 函数具有与普通文件系统操作类似的语义。

## 另请参阅 (SEE ALSO)

- `close()`
- `mmap()`
- `munmap()`
- `shmat()`
- `shmctl()`
- `shmdt()`
- `shm_open()`
- `<sys/mman.h>`

## 变更历史 (CHANGE HISTORY)

### 第 5 版首次发布
为与 POSIX 实时扩展对齐而包含。

### 第 6 版
`shm_unlink()` 函数被标记为共享内存对象选项的一部分。

在描述（DESCRIPTION）中，添加了文本以阐明在 `shm_unlink()` 后重用相同名称不会附加到旧的共享内存对象。

[ENOSYS] 错误条件已被移除，因为如果实现不支持共享内存对象选项，则不需要提供存根（stubs）。

### 第 7 版
应用 Austin Group 解释 1003.1-2001 #077，将 [ENAMETOOLONG] 从"应失败"错误更改为"可能失败"错误。

应用 Austin Group 解释 1003.1-2001 #141，添加未来方向（FUTURE DIRECTIONS）。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*