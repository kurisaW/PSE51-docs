# fputc — put a byte on a stream

## SYNOPSIS

```c
#include <stdio.h>

int fputc(int c, FILE *stream);
```

## DESCRIPTION

The `fputc()` function shall write the byte specified by `c` (converted to an `unsigned char`) to the output stream pointed to by `stream`, at the position indicated by the associated file-position indicator for the stream (if defined), and shall advance the indicator appropriately. If the file cannot support positioning requests, or if the stream was opened with append mode, the byte shall be appended to the output stream.

The last data modification and last file status change timestamps of the file shall be marked for update between the successful execution of `fputc()` and the next successful completion of a call to `fflush()` or `fclose()` on the same stream or a call to `exit()` or `abort()`.

## RETURN VALUE

Upon successful completion, `fputc()` shall return the value it has written. Otherwise, it shall return EOF, the error indicator for the stream shall be set, and `errno` shall be set to indicate the error.

## ERRORS

The `fputc()` function shall fail if either the `stream` is unbuffered or the `stream`'s buffer needs to be flushed, and:

- **[EAGAIN]**
  The O_NONBLOCK flag is set for the file descriptor underlying `stream` and the thread would be delayed in the write operation.

- **[EBADF]**
  The file descriptor underlying `stream` is not a valid file descriptor open for writing.

- **[EFBIG]**
  An attempt was made to write to a file that exceeds the maximum file size.

- **[EFBIG]**
  An attempt was made to write to a file that exceeds the file size limit of the process. A SIGXFSZ signal shall also be generated for the thread.

- **[EFBIG]**
  The file is a regular file and an attempt was made to write at or beyond the offset maximum.

- **[EINTR]**
  The write operation was terminated due to the receipt of a signal, and no data was transferred.

- **[EIO]**
  A physical I/O error has occurred, or the process is a member of a background process group attempting to write to its controlling terminal, TOSTOP is set, the calling thread is not blocking SIGTTOU, the process is not ignoring SIGTTOU, and the process group of the process is orphaned. This error may also be returned under implementation-defined conditions.

- **[ENOSPC]**
  There was no free space remaining on the device containing the file.

- **[EPIPE]**
  An attempt is made to write to a pipe or FIFO that is not open for reading by any process. A SIGPIPE signal shall also be sent to the thread.

The `fputc()` function may fail if:

- **[ENOMEM]**
  Insufficient storage space is available.

- **[ENXIO]**
  A request was made of a nonexistent device, or the request was outside the capabilities of the device.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

2.5 Standard I/O Streams, `ferror()`, `fopen()`, `getrlimit()`, `putc()`, `puts()`, `setbuf()`

XBD `<stdio.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

Large File Summit extensions are added.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The [EIO] and [EFBIG] mandatory error conditions are added.
- The [ENOMEM] and [ENXIO] optional error conditions are added.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/37 is applied, updating the [EAGAIN] error in the ERRORS section from "the process would be delayed" to "the thread would be delayed".

### Issue 7

Changes are made related to support for finegrained timestamps.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0168 [79] and XSH/TC1-2008/0169 [14] are applied.

### Issue 8

Austin Group Defect 308 is applied, clarifying the handling of [EFBIG] errors.

Austin Group Defect 1669 is applied, removing XSI shading from part of the [EFBIG] error relating to the file size limit for the process.