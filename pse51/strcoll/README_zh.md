# strcoll

## NAME

strcoll, strcoll_l — 使用排序信息的字符串比较

## SYNOPSIS

```c
#include <string.h>

int strcoll(const char *s1, const char *s2);

int strcoll_l(const char *s1, const char *s2, locale_t locale);
```

## DESCRIPTION

对于 `strcoll()`：本参考页描述的功能与 ISO C 标准保持一致。此处描述的要求与 ISO C 标准之间的任何冲突都是无意的。POSIX.1-2024 此卷遵循 ISO C 标准。

`strcoll()` 和 `strcoll_l()` 函数应将 `s1` 指向的字符串与 `s2` 指向的字符串进行比较，两个字符串分别根据当前 locale 的 `LC_COLLATE` 类别或 `locale` 表示的 locale 进行适当的解释。

如果成功，`strcoll()` 和 `strcoll_l()` 函数不应更改 `errno` 的设置。

由于没有保留返回值来指示错误，希望检查错误情况的应用程序应将 `errno` 设置为 0，然后调用 `strcoll()` 或 `strcoll_l()`，然后检查 `errno`。

如果 `strcoll_l()` 的 `locale` 参数是特殊的 locale 对象 LC_GLOBAL_LOCALE 或不是有效的 locale 对象句柄，则行为未定义。

## RETURN VALUE

成功完成后，`strcoll()` 应返回一个大于、等于或小于 0 的整数，具体取决于当 `s1` 指向的字符串在根据当前 locale 进行适当解释时，是大于、等于还是小于 `s2` 指向的字符串。出错时，`strcoll()` 可以设置 `errno`，但没有保留返回值来指示错误。

成功完成后，`strcoll_l()` 应返回一个大于、等于或小于 0 的整数，具体取决于当 `s1` 指向的字符串在根据 `locale` 表示的 locale 进行适当解释时，是大于、等于还是小于 `s2` 指向的字符串。出错时，`strcoll_l()` 可以设置 `errno`，但没有保留返回值来指示错误。

## ERRORS

这些函数可能在以下情况下失败：

- **[EINVAL]**
  - `s1` 或 `s2` 参数包含排序序列域之外的字符。

## EXAMPLES

### 比较节点

以下示例使用应用程序定义的函数 `node_compare()` 来基于 `string` 字段的字母顺序比较两个节点。

```c
#include <string.h>
...
struct node { /* 这些存储在表中。 */
    char *string;
    int length;
};
...
int node_compare(const void *node1, const void *node2)
{
    return strcoll(((const struct node *)node1)->string,
        ((const struct node *)node2)->string);
}
...
```

## APPLICATION USAGE

`strxfrm()` 和 `strcmp()` 函数应该用于对大型列表进行排序。

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- `alphasort()`
- `strcmp()`
- `strxfrm()`

XBD `<string.h>`

## CHANGE HISTORY

### 首次发布于 Issue 3。

### Issue 5

更新 DESCRIPTION 以指示如果函数成功则 `errno` 不会更改。

### Issue 6

标记了超出 ISO C 标准的扩展。

以下对 POSIX 实现的新要求源于与单一 UNIX 规范的对齐：

- 添加了 [EINVAL] 可选错误条件。

添加了示例。

### Issue 7

从 The Open Group Technical Standard, 2006, Extended API Set Part 4 添加了 `strcoll_l()` 函数。

应用了 POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0593 [283] 和 XSH/TC1-2008/0594 [283]。

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*