# perror

## SYNOPSIS

```c
#include <stdio.h>

void perror(const char *s);
```

## DESCRIPTION

`perror()` 函数应将通过符号 `errno` 访问的错误号映射到与语言相关的错误消息，该消息应按以下格式写入标准错误流：

* 首先（如果 `s` 不是空指针且 `s` 指向的字符不是空字节），写入 `s` 指向的字符串，后跟一个 `<冒号>` 和一个 `<空格>`。
* 然后是错误消息字符串，后跟一个 `<换行符>`。

错误消息字符串的内容应与 `strerror()` 函数以 `errno` 为参数返回的内容相同。

`perror()` 函数应在成功完成与 `exit()`、`abort()` 或在 `stderr` 上完成 `fflush()` 或 `fclose()` 之间的某个时间，标记与标准错误流关联的文件的数据修改和文件状态更改的最后时间戳以供更新。

`perror()` 函数不应改变标准错误流的方向。

出错时，`perror()` 应为 `stderr` 指向的流设置错误指示器，并应设置 `errno` 以指示错误。

由于不返回任何值，希望检查错误情况的应用程序应在调用 `perror()` 之前调用 `clearerr(stderr)`，然后如果 `ferror(stderr)` 返回非零值，`errno` 的值指示发生了哪个错误。

## RETURN VALUE

`perror()` 函数不应返回任何值。

## ERRORS

参考 `fputc()`。

---

## EXAMPLES

### 为函数打印错误消息

以下示例将 `bufptr` 替换为具有必要大小的缓冲区。如果发生错误，`perror()` 函数打印一条消息且程序退出。

```c
#include <stdio.h>
#include <stdlib.h>
...
char *bufptr;
size_t szbuf;
...
if ((bufptr = malloc(szbuf)) == NULL) {
    perror("malloc"); exit(2);
}
...
```

## APPLICATION USAGE

应用程序编写者可能更喜欢使用替代接口而不是 `perror()`，例如将 `strerror_r()` 与 `fprintf()` 结合使用。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `fprintf()`
- `fputc()`
- `psiginfo()`
- `strerror()`
- `<stdio.h>`

## CHANGE HISTORY

**首次发布**于 Issue 1。源自 SVID 的 Issue 1。

**Issue 5**：向 DESCRIPTION 添加了一段，指出 `perror()` 不改变标准错误流的方向。

**Issue 6**：标记了超出 ISO C 标准的扩展。

**Issue 7**：应用了 SD5-XSH-ERN-95。进行了与支持细粒度时间戳相关的更改。应用了 POSIX.1-2008、Technical Corrigendum 1、XSH/TC1-2008/0429 [389,401]、XSH/TC1-2008/0430 [389] 和 XSH/TC1-2008/0431 [389,401]。

---

*UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved*