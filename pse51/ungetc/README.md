# ungetc — push byte back into input stream

## SYNOPSIS

```c
#include <stdio.h>

int ungetc(int c, FILE *stream);
```

## DESCRIPTION

The `ungetc()` function shall push the byte specified by `c` (converted to an **unsigned char**) back onto the input stream pointed to by `stream`. The pushed-back bytes shall be returned by subsequent reads on that stream in the reverse order of their pushing. A successful intervening call (with the stream pointed to by `stream`) to a file-positioning function ([`fseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html), [`fseeko()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseeko.html), [`fsetpos()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsetpos.html), or [`rewind()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/rewind.html)) or [`fflush()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html) shall discard any pushed-back bytes for the stream. The external storage corresponding to the stream shall be unchanged.

One byte of push-back shall be provided. If `ungetc()` is called too many times on the same stream without an intervening read or file-positioning operation on that stream, the operation may fail.

If the value of `c` equals that of the macro EOF, the operation shall fail and the input stream shall be left unchanged.

A successful call to `ungetc()` shall clear the end-of-file indicator for the stream. The file-position indicator for the stream shall be decremented by each successful call to `ungetc()`; if its value was 0 before a call, its value is unspecified after the call. The value of the file-position indicator after all pushed-back bytes have been read shall be the same as it was before the bytes were pushed back.

## RETURN VALUE

Upon successful completion, `ungetc()` shall return the byte pushed back after conversion. Otherwise, it shall return EOF.

## ERRORS

No errors are defined.

## APPLICATION USAGE

None.

## RATIONALE

The ISO C standard includes the text "The value of the file position indicator for the stream after reading or discarding all pushed-back characters shall be the same as it was before the characters were pushed back." POSIX.1 omits "or discarding" from this because it is redundant—in the ISO C standard the discarding is done by file positioning functions and does not affect the position set by those functions. In particular, a relative seek using [`fseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html) or [`fseeko()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseeko.html) with SEEK_CUR adjusts the position relative to the position on entry to the function, not the position after the pushed-back bytes have been discarded. POSIX.1 also requires [`fflush()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html) to discard pushed back bytes in situations where the ISO C standard says the behavior of [`fflush()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fflush.html) is undefined.

## FUTURE DIRECTIONS

The ISO C standard states that the use of `ungetc()` on a binary stream where the file position indicator is zero prior to the call is an obsolescent feature. In POSIX.1 there is no distinction between binary and text streams, so this applies to all streams. This feature may be removed in a future version of this standard.

## SEE ALSO

- [2.5 Standard I/O Streams](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_05)
- [`fseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fseek.html)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [`fsetpos()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsetpos.html)
- [`read()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html#tag_17_476)
- [`rewind()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/rewind.html)
- [`setbuf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/setbuf.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 7

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0687 [87,93], XSH/TC1-2008/0688 [87], and XSH/TC1-2008/0689 [14] are applied.

### Issue 8

Austin Group Defect 701 is applied, clarifying how the file-position indicator for the stream is updated.

Austin Group Defect 1302 is applied, changing the FUTURE DIRECTIONS section.