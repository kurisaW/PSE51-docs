# fgetc — get a byte from a stream

## SYNOPSIS

```c
#include <stdio.h>

int fgetc(FILE *stream);
```

## DESCRIPTION

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

If the end-of-file indicator for the input stream pointed to by `stream` is not set and a next byte is present, the `fgetc()` function shall obtain the next byte as an **unsigned char** converted to an **int**, from the input stream pointed to by `stream`, and advance the associated file position indicator for the stream (if defined). Since `fgetc()` operates on bytes, reading a character consisting of multiple bytes (or "a multi-byte character") may require multiple calls to `fgetc()`.

The `fgetc()` function may mark the last data access timestamp of the file associated with `stream` for update. The last data access timestamp shall be marked for update by the first successful execution of `fgetc()`, `fgets()`, `fread()`, `fscanf()`, `getc()`, `getchar()`, `getdelim()`, `getline()`, or `scanf()` using `stream` that returns data not supplied by a prior call to `ungetc()`.

## RETURN VALUE

Upon successful completion, `fgetc()` shall return the next byte from the input stream pointed to by `stream`. If the end-of-file indicator for the stream is set, or if the stream is at end-of-file, the end-of-file indicator for the stream shall be set and `fgetc()` shall return EOF. If an error occurs, the error indicator for the stream shall be set, `fgetc()` shall return EOF, and shall set `errno` to indicate the error.

## ERRORS

The `fgetc()` function shall fail if data needs to be read and:

- **[EAGAIN]** The O_NONBLOCK flag is set for the file descriptor underlying `stream` and the thread would be delayed in the `fgetc()` operation.

- **[EBADF]** The file descriptor underlying `stream` is not a valid file descriptor open for reading.

- **[EINTR]** The read operation was terminated due to the receipt of a signal, and no data was transferred.

- **[EIO]** A physical I/O error has occurred, or the process is in a background process group attempting to read from its controlling terminal, and either the calling thread is blocking SIGTTIN or the process is ignoring SIGTTIN or the process group of the process is orphaned. This error may also be generated for implementation-defined reasons.

- **[EOVERFLOW]** The file is a regular file and an attempt was made to read at or beyond the offset maximum associated with the corresponding stream.

The `fgetc()` function may fail if:

- **[ENOMEM]** Insufficient storage space is available.

- **[ENXIO]** A request was made of a nonexistent device, or the request was outside the capabilities of the device.

## EXAMPLES

None.

## APPLICATION USAGE

If the integer value returned by `fgetc()` is stored into a variable of type **char** and then compared against the integer constant EOF, the comparison may never succeed, because sign-extension of a variable of type **char** on widening to integer is implementation-defined.

The `ferror()` or `feof()` functions must be used to distinguish between an error condition and an end-of-file condition.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- 2.5 Standard I/O Streams
- [`feof()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/feof.html)
- [`ferror()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ferror.html)
- [`fgets()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fgets.html)
- [`fread()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fread.html)
- [`fscanf()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fscanf.html)
- [`getchar()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getchar.html)
- [`getc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/getc.html)
- [`ungetc()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/ungetc.html)
- XBD [`<stdio.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/stdio.h.html)

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5
Large File Summit extensions are added.

### Issue 6
Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:
- The [EIO] and [EOVERFLOW] mandatory error conditions are added.
- The [ENOMEM] and [ENXIO] optional error conditions are added.

The following changes are made for alignment with the ISO/IEC 9899:1999 standard:
- The DESCRIPTION is updated to clarify the behavior when the end-of-file indicator for the input stream is not set.
- The RETURN VALUE section is updated to note that the error indicator shall be set for the stream.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/32 is applied, updating the [EAGAIN] error in the ERRORS section from "the process would be delayed" to "the thread would be delayed".

### Issue 7
Austin Group Interpretation 1003.1-2001 #051 is applied, updating the list of functions that mark the last data access timestamp for update.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0129 [79] and XSH/TC1-2008/0130 [14] are applied.

### Issue 8
Austin Group Defect 1330 is applied, removing obsolescent interfaces.

Austin Group Defect 1624 is applied, changing the RETURN VALUE section.