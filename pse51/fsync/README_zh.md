# fsync

## SYNOPSIS

```c
#include <unistd.h>

int fsync(int fildes);
```

## DESCRIPTION

`fsync()` 函数应当请求将 `fildes` 指定的打开文件描述符的所有数据传输到与该文件关联的存储设备。传输的性质由实现定义。`fsync()` 函数在系统完成该操作或检测到错误之前不应返回。

如果定义了 `_POSIX_SYNCHRONIZED_IO`，`fsync()` 函数应强制将与文件描述符 `fildes` 关联的所有当前排队的 I/O 操作转为同步 I/O 完成状态。所有 I/O 操作应按照同步 I/O 文件完整性完成的规定完成。

## RETURN VALUE

成功完成后，`fsync()` 应返回 0。否则，应返回 -1 并设置 `errno` 以指示错误。如果 `fsync()` 函数失败，不保证未完成的 I/O 操作已完成。

## ERRORS

`fsync()` 函数在以下情况下应失败：

- **[EBADF]**
  `fildes` 参数不是有效的描述符。

- **[EINTR]**
  `fsync()` 函数被信号中断。

- **[EINVAL]**
  `fildes` 参数不指向支持此操作的文件。

- **[EIO]**
  在从文件系统读取或写入时发生 I/O 错误。

如果任何排队的 I/O 操作失败，`fsync()` 应返回为 `read()` 和 `write()` 定义的错误条件。

*以下部分为补充信息。*

## EXAMPLES

无。

## APPLICATION USAGE

需要在继续执行前完成文件修改的程序应使用 `fsync()` 函数；例如，包含简单事务功能的程序可能使用它来确保事务引起的所有对文件的修改都被记录。

修改目录的应用程序（例如在目录中创建文件）可以对目录调用 `fsync()` 以确保目录条目和文件属性同步。对于大多数应用程序，不需要同步目录条目（参见 XBD 4.11 文件系统缓存）。

## RATIONALE

`fsync()` 函数旨在强制将数据从缓冲区缓存进行物理写入，并确保在系统崩溃或其他故障之后，截至 `fsync()` 调用时的所有数据都记录在磁盘上。由于"缓冲区缓存"、"系统崩溃"、"物理写入"和"非易失性存储"的概念在此处未定义，措辞必须更加抽象。

如果未定义 `_POSIX_SYNCHRONIZED_IO`，措辞在很大程度上依赖一致性文档来告知用户可以从系统预期什么。明确允许空实现。这在系统在任何情况下都无法保证非易失性存储，或者系统具有高度容错性且不需要此功能的情况下可能是有效的。在这些极端情况之间的中间地带，`fsync()` 可能或可能不会实际导致数据写入到能够抵御电源故障的位置。一致性文档应至少标识存在一种配置（以及如何获得该配置），其中至少可以确保用户可以选择用于关键数据的某些文件的这种安全。不要求提供详尽列表，而是提供足够的信息，以便在需要保存关键数据时，用户可以确定如何配置系统以允许数据写入非易失性存储。

可以合理地断言，`fsync()` 的关键方面在测试套件中测试是不合理的。这并不会使函数价值降低，只是使其更难测试。正式的一致性测试可能应该在此条件的测试期间强制系统崩溃（电源关闭），但需要以这种方式进行，使得自动化测试除在制作正式结果记录时之外不需要这样做。省略对 `fsync()` 的测试也是合理的，允许将其视为实现质量问题。

## FUTURE DIRECTIONS

无。

## SEE ALSO

[`sync()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sync.html)

XBD [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## CHANGE HISTORY

首次发布于 Issue 3。

### Issue 5

与 POSIX 实时扩展中的 `fsync()` 对齐。具体来说，DESCRIPTION 和 RETURN VALUE 部分大幅扩展，ERRORS 部分更新为指示 `fsync()` 可以返回为 `read()` 和 `write()` 定义的错误条件。

### Issue 6

此函数被标记为文件同步选项的一部分。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 添加了 `[EINVAL]` 和 `[EIO]` 强制错误条件。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/44，对 DESCRIPTION 进行了编辑性重新措辞。不打算改变含义。

### Issue 8

应用了 Austin Group 缺陷 672，更改了 APPLICATION USAGE 部分。

*补充信息结束。*

---
