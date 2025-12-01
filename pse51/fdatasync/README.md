# fdatasync

**The Open Group Base Specifications Issue 8**
IEEE Std 1003.1-2024
Copyright © 2001-2024 The IEEE and The Open Group

## NAME

fdatasync — synchronize the data of a file (**REALTIME**)

## SYNOPSIS

```c
#include <unistd.h>

int fdatasync(int fildes);
```

## DESCRIPTION

The `fdatasync()` function shall force all currently queued I/O operations associated with the file indicated by file descriptor `fildes` to the synchronized I/O completion state.

The functionality shall be equivalent to `fsync()` with the symbol _POSIX_SYNCHRONIZED_IO defined, with the exception that all I/O operations shall be completed as defined for synchronized I/O data integrity completion.

## RETURN VALUE

If successful, the `fdatasync()` function shall return the value 0; otherwise, the function shall return the value -1 and set `errno` to indicate the error. If the `fdatasync()` function fails, outstanding I/O operations are not guaranteed to have been completed.

## ERRORS

The `fdatasync()` function shall fail if:

- **[EBADF]**
  The `fildes` argument is not a valid file descriptor.

- **[EINVAL]**
  This implementation does not support synchronized I/O for this file.

In the event that any of the queued I/O operations fail, `fdatasync()` shall return the error conditions defined for `read()` and `write()`.

---

*The following sections are informative.*

## EXAMPLES

None.

## APPLICATION USAGE

Note that even if the file descriptor is not open for writing, if there are any pending write requests on the underlying file, then that I/O will be completed prior to the return of `fdatasync()`.

An application that modifies a directory, for example, by creating a file in the directory, can invoke `fdatasync()` on the directory to ensure that the directory's entries are synchronized, although for most applications this should not be necessary (see XBD [4.11 File System Cache](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap04.html#tag_04_11)).

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`aio_fsync()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/aio_fsync.html)
- [`fcntl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fcntl.html)
- [`fsync()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fsync.html)
- [`open()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/open.html)
- [`read()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html)
- [`write()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/write.html)
- XBD [`<unistd.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/unistd.h.html)

## CHANGE HISTORY

**First released in Issue 5.** Included for alignment with the POSIX Realtime Extension.

### Issue 6

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Synchronized Input and Output option.

The `fdatasync()` function is marked as part of the Synchronized Input and Output option.

### Issue 7

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0110 [501] is applied.

### Issue 8

Austin Group Defect 672 is applied, changing the APPLICATION USAGE section.

*End of informative text.*

---

UNIX® is a registered Trademark of The Open Group.
POSIX™ is a Trademark of The IEEE.
Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved