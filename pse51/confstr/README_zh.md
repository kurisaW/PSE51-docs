# confstr — 获取可配置字符串变量

## 概要 (SYNOPSIS)

```c
#include <unistd.h>

size_t confstr(int name, char *buf, size_t len);
```

## 描述 (DESCRIPTION)

`confstr()` 函数应返回配置定义的字符串值。其用途和目的与 `sysconf()` 类似，但用于返回字符串值而非数值。

`name` 参数表示要查询的系统变量。实现应支持以下在 `<unistd.h>` 中定义的 name 值，也可以支持其他值：

- `_CS_PATH`
- `_CS_POSIX_V8_ILP32_OFF32_CFLAGS`
- `_CS_POSIX_V8_ILP32_OFF32_LDFLAGS`
- `_CS_POSIX_V8_ILP32_OFF32_LIBS`
- `_CS_POSIX_V8_ILP32_OFFBIG_CFLAGS`
- `_CS_POSIX_V8_ILP32_OFFBIG_LDFLAGS`
- `_CS_POSIX_V8_ILP32_OFFBIG_LIBS`
- `_CS_POSIX_V8_LP64_OFF64_CFLAGS`
- `_CS_POSIX_V8_LP64_OFF64_LDFLAGS`
- `_CS_POSIX_V8_LP64_OFF64_LIBS`
- `_CS_POSIX_V8_LPBIG_OFFBIG_CFLAGS`
- `_CS_POSIX_V8_LPBIG_OFFBIG_LDFLAGS`
- `_CS_POSIX_V8_LPBIG_OFFBIG_LIBS`
- `_CS_POSIX_V8_THREADS_CFLAGS`
- `_CS_POSIX_V8_THREADS_LDFLAGS`
- `_CS_POSIX_V8_WIDTH_RESTRICTED_ENVS`
- `_CS_V8_ENV`

[OB] `_CS_POSIX_V7_ILP32_OFF32_CFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFF32_LDFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFF32_LIBS`
[OB] `_CS_POSIX_V7_ILP32_OFFBIG_CFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFFBIG_LDFLAGS`
[OB] `_CS_POSIX_V7_ILP32_OFFBIG_LIBS`
[OB] `_CS_POSIX_V7_LP64_OFF64_CFLAGS`
[OB] `_CS_POSIX_V7_LP64_OFF64_LDFLAGS`
[OB] `_CS_POSIX_V7_LP64_OFF64_LIBS`
[OB] `_CS_POSIX_V7_LPBIG_OFFBIG_CFLAGS`
[OB] `_CS_POSIX_V7_LPBIG_OFFBIG_LDFLAGS`
[OB] `_CS_POSIX_V7_LPBIG_OFFBIG_LIBS`
[OB] `_CS_POSIX_V7_THREADS_CFLAGS`
[OB] `_CS_POSIX_V7_THREADS_LDFLAGS`
[OB] `_CS_POSIX_V7_WIDTH_RESTRICTED_ENVS`
[OB] `_CS_V7_ENV`

如果 `len` 不为 0，且 `name` 有配置定义的值，`confstr()` 应将该值复制到由 `buf` 指向的长度为 `len` 字节的缓冲区中。如果要返回的字符串长度（包括终止空字符）超过 `len` 字节，那么 `confstr()` 应将字符串截断为 `len-1` 字节并将结果空字符终止。应用程序可以通过比较 `confstr()` 返回的值与 `len` 来检测字符串是否被截断。

如果 `len` 为 0 且 `buf` 为空指针，那么 `confstr()` 仍应返回如下定义的整数值，但不返回字符串。如果 `len` 为 0 但 `buf` 不是空指针，则结果未指定。

在调用以下代码后：

```c
confstr(_CS_V8_ENV, buf, sizeof(buf))
```

存储在 `buf` 中的字符串应包含空格分隔的 variable=值 环境变量对列表，这些变量是实现作为指定符合性环境的一部分所需要的，如实现的符合性文档中所述。

如果实现支持 POSIX shell 选项，在调用以下代码后存储在 `buf` 中的字符串：

```c
confstr(_CS_PATH, buf, sizeof(buf))
```

可以用作 `PATH` 环境变量的值来访问 POSIX.1-2024 的所有标准实用程序，这些实用程序的提供方式可通过 `exec` 函数族访问，前提是返回值小于或等于 `sizeof(buf)`。

## 返回值 (RETURN VALUE)

如果 `name` 有配置定义的值，`confstr()` 应返回保存整个配置定义值（包括终止空字符）所需的缓冲区大小。如果此返回值大于 `len`，则返回在 `buf` 中的字符串被截断。

如果 `name` 无效，`confstr()` 应返回 0 并设置 `errno` 以指示错误。

如果 `name` 没有配置定义的值，`confstr()` 应返回 0 且保持 `errno` 不变。

## 错误 (ERRORS)

如果发生以下情况，`confstr()` 函数应失败：

- **[EINVAL]**
  - `name` 参数的值无效。

---

*以下各节为资料性内容。*

## 示例 (EXAMPLES)

无。

## 应用程序用法 (APPLICATION USAGE)

应用程序可以通过检查 `errno` 是否被修改来区分无效的 `name` 参数值和对应于没有配置定义值但可配置变量。这镜像了 `sysconf()` 的行为。

此函数的最初需求是提供一种方法来查找环境变量 `PATH` 的配置定义默认值。由于 `PATH` 可以被用户修改为包含可能包含替换 POSIX.1-2024 Shell 和实用程序卷中标准实用程序的实用程序的目录，应用程序需要一种方法来确定系统提供的 `PATH` 环境变量值，该值包含标准实用程序的正确搜索路径。

应用程序可以使用：

```c
confstr(name, (char *)NULL, (size_t)0)
```

来找出字符串值需要多大的缓冲区；使用 `malloc()` 分配缓冲区来保存字符串；然后再次调用 `confstr()` 来获取字符串。或者，它可以分配一个足够大的固定静态缓冲区来保存大多数答案（可能是 512 或 1024 字节），但如果发现这个缓冲区太小，就使用 `malloc()` 分配更大的缓冲区。

## 基本原理 (RATIONALE)

应用程序开发人员通常可以通过读取由以下调用打开的流来确定任何配置变量：

```c
popen("command -p getconf variable", "r");
```

`confstr()` 函数在 `name` 参数为 `_CS_PATH` 时返回一个字符串，该字符串可用作 `PATH` 环境变量设置，将引用 POSIX.1-2024 Shell 和实用程序卷中描述的标准 shell 和实用程序。

`confstr()` 函数将返回的字符串复制到应用程序提供的缓冲区中，而不是返回指向字符串的指针。这在某些实现（如具有轻量级线程的实现）中提供了更清洁的函数，并解决了应用程序何时必须复制返回字符串的问题。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 另请参见 (SEE ALSO)

- [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html)
- [`fpathconf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fpathconf.html)
- [`sysconf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sysconf.html)

XBD `<unistd.h>`

XCU `c17`

## 变更历史 (CHANGE HISTORY)

首次在 Issue 4 中发布。源自 ISO POSIX-2 标准。

### Issue 5

在描述中添加了表示 `name` 允许值的表格。所有标记为 EX 的值在此版本中是新的。

### Issue 6

应用了 The Open Group Corrigendum U033/7。返回缓冲区大小情况的返回值现在明确说明这包括终止空字符。

从与单一 UNIX 规范对齐得出以下对 POSIX 实现的新要求：

- 描述中更新了新参数，这些参数可用于确定每个不同支持编程环境的 C 编译器标志、链接器/加载器标志和库的配置字符串。这是为了支持数据大小中立性的更改。

做出了以下更改以与 IEEE P1003.1a 草案标准对齐：

- 描述中更新了文本，描述了如何使用 `_CS_PATH` 获取访问标准实用程序的 `PATH`。

与 `c89` 编程模型关联的宏被标记为 LEGACY，并引入了与 `c99` 关联的新等效宏。

### Issue 7

应用了 Austin Group Interpretation 1003.1-2001 #047，添加了 `_CS_V7_ENV` 变量。

应用了 Austin Group Interpretations 1003.1-2001 #166 以允许额外的编译器标志来启用线程。

支持的编程环境的 V6 变量被标记为过时的。

支持的编程环境的变量被更新为 V7。

LEGACY 变量和过时值被移除。

应用了 POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0070 [810] 和 XSH/TC2-2008/0071 [911]。

### Issue 8

应用了 Austin Group Defect 1330，将 "_V7_" 更改为 "_V8_"，将 "_V6_" 更改为 "_V7_"。

---
