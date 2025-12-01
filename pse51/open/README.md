# open, openat — open file

## SYNOPSIS

```c
#include <sys/stat.h>
#include <fcntl.h>

int open(const char *path, int oflag, ...);
int openat(int fd, const char *path, int oflag, ...);
```

## DESCRIPTION

The `open()` function shall establish the connection between a file and a file descriptor. It shall create an open file description that refers to a file and a file descriptor that refers to that open file description. The file descriptor is used by other I/O functions to refer to that file. The `path` argument points to a pathname naming the file.

The `open()` function shall return a file descriptor for the named file, allocated as described in [2.6 File Descriptor Allocation](https://pubs.opengroup.org/onlinepubs/9799919799/functions/V2_chap02.html#tag_16_06). The open file description is new, and therefore the file descriptor shall not share it with any other process in the system. The `FD_CLOEXEC` file descriptor flag associated with the new file descriptor shall be cleared unless the `O_CLOEXEC` flag is set in `oflag`. The `FD_CLOFORK` file descriptor flag associated with the new file descriptor shall be cleared unless the `O_CLOFORK` flag is set in `oflag`.

The file offset used to mark the current position within the file shall be set to the beginning of the file.

The file status flags and file access modes of the open file description shall be set according to the value of `oflag`.

Values for `oflag` are constructed by a bitwise-inclusive OR of flags from the following list, defined in `<fcntl.h>`. Applications shall specify exactly one of the first five values (file access modes) below in the value of `oflag`:

### File Access Modes

- **O_EXEC**: Open for execute only (non-directory files). If `path` names a directory and `O_EXEC` is not the same value as `O_SEARCH`, `open()` shall fail.

- **O_RDONLY**: Open for reading only.

- **O_RDWR**: Open for reading and writing. If `path` names a FIFO, and the implementation does not support opening a FIFO for simultaneous read and write, then `open()` shall fail.

- **O_SEARCH**: Open directory for search only. If `path` names a non-directory file and `O_SEARCH` is not the same value as `O_EXEC`, `open()` shall fail.

- **O_WRONLY**: Open for writing only.

### Additional Flags

Any combination of the following may be used:

- **O_APPEND**: If set, the file offset shall be set to the end of the file prior to each write.

- **O_CLOEXEC**: If set, the `FD_CLOEXEC` flag for the new file descriptor shall be set.

- **O_CLOFORK**: If set, the `FD_CLOFORK` flag for the new file descriptor shall be set.

- **O_CREAT**: If the file exists, this flag has no effect except as noted under `O_EXCL` below. Otherwise, if `O_DIRECTORY` is not set the file shall be created as a regular file; the user ID of the file shall be set to the effective user ID of the process; the group ID of the file shall be set to the group ID of the file's parent directory or to the effective group ID of the process; and the access permission bits (see `<sys/stat.h>`) of the file mode shall be set to the value of the argument following the `oflag` argument taken as type `mode_t` modified as follows: a bitwise AND is performed on the file-mode bits and the corresponding bits in the complement of the process' file mode creation mask.

- **O_DIRECTORY**: If `path` resolves to a non-directory file, fail and set `errno` to `[ENOTDIR]`.

- **O_DSYNC** [SIO]: Write I/O operations on the file descriptor shall complete as defined by synchronized I/O data integrity completion.

- **O_EXCL**: If `O_CREAT` and `O_EXCL` are set, `open()` shall fail if the file exists. The check for the existence of the file and the creation of the file if it does not exist shall be atomic with respect to other threads executing `open()` naming the same filename in the same directory with `O_EXCL` and `O_CREAT` set. If `O_EXCL` and `O_CREAT` are set, and `path` names a symbolic link, `open()` shall fail and set `errno` to `[EEXIST]`, regardless of the contents of the symbolic link.

- **O_NOCTTY**: If set and `path` identifies a terminal device, `open()` shall not cause the terminal device to become the controlling terminal for the process.

- **O_NOFOLLOW**: If `path` names a symbolic link, fail and set `errno` to `[ELOOP]`.

- **O_NONBLOCK**: When opening a FIFO with `O_RDONLY` or `O_WRONLY` set:
  - If `O_NONBLOCK` is set, an `open()` for reading-only shall return without delay. An `open()` for writing-only shall return an error if no process currently has the file open for reading.
  - If `O_NONBLOCK` is clear, an `open()` for reading-only shall block the calling thread until a thread opens the file for writing. An `open()` for writing-only shall block the calling thread until a thread opens the file for reading.

  When opening a block special or character special file that supports non-blocking opens:
  - If `O_NONBLOCK` is set, the `open()` function shall return without blocking for the device to be ready or available.
  - If `O_NONBLOCK` is clear, the `open()` function shall block the calling thread until the device is ready or available before returning.

- **O_RSYNC** [SIO]: Read I/O operations on the file descriptor shall complete at the same level of integrity as specified by the `O_DSYNC` and `O_SYNC` flags.

- **O_SYNC** [XSI|SIO]: Write I/O operations on the file descriptor shall complete as defined by synchronized I/O file integrity completion. The `O_SYNC` flag shall be supported for regular files, even if the Synchronized Input and Output option is not supported.

- **O_TRUNC**: If the file exists and is a regular file, and the file is successfully opened `O_RDWR` or `O_WRONLY`, its length shall be truncated to 0, and the mode and owner shall be unchanged.

- **O_TTY_INIT**: If `path` identifies a terminal device other than a pseudo-terminal, the device is not already open in any process, and either `O_TTY_INIT` is set in `oflag` or `O_TTY_INIT` has the value zero, `open()` shall set any non-standard `termios` structure terminal parameters to a state that provides conforming behavior and initialize the `winsize` structure associated with the terminal to appropriate default settings.

The `openat()` function shall be equivalent to the `open()` function except in the case where `path` specifies a relative path. In this case the file to be opened is determined relative to the directory associated with the file descriptor `fd` instead of the current working directory.

If `openat()` is passed the special value `AT_FDCWD` in the `fd` parameter, the current working directory shall be used and the behavior shall be identical to a call to `open()`.

## RETURN VALUE

Upon successful completion, these functions shall open the file and return a non-negative integer representing the file descriptor. Otherwise, these functions shall return -1 and set `errno` to indicate the error. If -1 is returned, no files shall be created or modified.

## ERRORS

These functions shall fail if:

- **[EACCES]**: Search permission is denied on a component of the path prefix, or the file exists and the permissions specified by `oflag` are denied, or the file does not exist and write permission is denied for the parent directory of the file to be created, or `O_TRUNC` is specified and write permission is denied.

- **[EEXIST]**: `O_CREAT` and `O_EXCL` are set, and the named file exists.

- **[EILSEQ]**: `O_CREAT` was specified, the file did not exist, and the last pathname component of `path` is not a portable filename and cannot be created in the target directory.

- **[EINTR]**: A signal was caught during `open()`.

- **[EINVAL]**: The `path` argument names a FIFO, `O_RDWR` was specified, and the implementation considers this an error; or synchronized I/O flags were specified and the implementation does not support synchronized I/O for the file.

- **[EISDIR]**: The named file is a directory and `oflag` includes `O_WRONLY` or `O_RDWR`, or includes `O_CREAT` without `O_DIRECTORY`, or includes `O_EXEC` when `O_EXEC` is not the same value as `O_SEARCH`.

- **[ELOOP]**: A loop exists in symbolic links encountered during resolution of the `path` argument, or `O_NOFOLLOW` was specified and the `path` argument names a symbolic link.

- **[EMFILE]**: All file descriptors available to the process are currently open.

- **[ENAMETOOLONG]**: The length of a component of a pathname is longer than `{NAME_MAX}`.

- **[ENFILE]**: The maximum allowable number of files is currently open in the system.

- **[ENOENT]**: `O_CREAT` is not set and a component of `path` does not name an existing file, or `O_CREAT` is set and a component of the path prefix of `path` does not name an existing file, or `path` points to an empty string.

- **[ENOENT]** or **[ENOTDIR]**: `O_CREAT` is set, and the `path` argument contains at least one non-`<slash>` character and ends with one or more trailing `<slash>` characters.

- **[ENOSPC]**: The directory or file system that would contain the new file cannot be expanded, the file does not exist, and `O_CREAT` is specified.

- **[ENOTDIR]**: A component of the path prefix names an existing file that is neither a directory nor a symbolic link to a directory; or `O_CREAT` and `O_EXCL` are not specified, the `path` argument contains at least one non-`<slash>` character and ends with one or more trailing `<slash>` characters; or `O_DIRECTORY` was specified and the `path` argument names a non-directory file.

- **[ENXIO]**: `O_NONBLOCK` is set, the named file is a FIFO, `O_WRONLY` is set, and no process has the file open for reading.

- **[ENXIO]**: The named file is a character special or block special file, and the device associated with this special file does not exist.

- **[EOVERFLOW]**: The named file is a regular file and the size of the file cannot be represented correctly in an object of type `off_t`.

- **[EROFS]**: The named file resides on a read-only file system and either `O_WRONLY`, `O_RDWR`, `O_CREAT` (if the file does not exist), or `O_TRUNC` is set in the `oflag` argument.

The `openat()` function shall fail if:

- **[EACCES]**: The access mode of the open file description associated with `fd` is not `O_SEARCH` and the permissions of the directory underlying `fd` do not permit directory searches.

- **[EBADF]**: The `path` argument does not specify an absolute path and the `fd` argument is neither `AT_FDCWD` nor a valid file descriptor open for reading or searching.

- **[ENOTDIR]**: The `path` argument is not an absolute path and `fd` is a file descriptor associated with a non-directory file.

These functions may fail if:

- **[EAGAIN]** [XSI]: The `path` argument names the subsidiary side of a pseudo-terminal device that is locked.

- **[EINVAL]**: The value of the `oflag` argument is not valid.

- **[ELOOP]**: More than `{SYMLOOP_MAX}` symbolic links were encountered during resolution of the `path` argument.

- **[ENAMETOOLONG]**: The length of a pathname exceeds `{PATH_MAX}`, or pathname resolution of a symbolic link produced an intermediate result with a length that exceeds `{PATH_MAX}`.

- **[EOPNOTSUPP]**: The `path` argument names a socket.

- **[ETXTBSY]**: The file is a pure procedure (shared text) file that is being executed and `oflag` is `O_WRONLY` or `O_RDWR`.

## EXAMPLES

### Opening a File for Writing by the Owner

```c
#include <fcntl.h>
...
int fd;
mode_t mode = S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH;
char *pathname = "/tmp/file";
...
fd = open(pathname, O_WRONLY | O_CREAT | O_TRUNC, mode);
...
```

### Opening a File Using an Existence Check

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>

#define LOCKFILE "/etc/ptmp"
...
int pfd; /* Integer for file descriptor returned by open() call. */
...
if ((pfd = open(LOCKFILE, O_WRONLY | O_CREAT | O_EXCL,
    S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH)) == -1)
{
    fprintf(stderr, "Cannot open /etc/ptmp. Try again later.\n");
    exit(1);
}
...
```

### Opening a File for Writing

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>

#define LOCKFILE "/etc/ptmp"
...
int pfd;
char pathname[PATH_MAX+1];
...
if ((pfd = open(pathname, O_WRONLY | O_CREAT | O_TRUNC,
    S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH)) == -1)
{
    perror("Cannot open output file\n");
    exit(1);
}
...
```

## APPLICATION USAGE

POSIX.1-2024 does not require that terminal parameters be automatically set to any state on first open, nor that they be reset after the last close. To ensure that the device is set to a conforming initial state, applications which perform a first open of a terminal (other than a pseudo-terminal) should do so using the `O_TTY_INIT` flag.

Except as specified in this volume of POSIX.1-2024, the flags allowed in `oflag` are not mutually-exclusive and any number of them may be used simultaneously.

The `O_CLOEXEC` and `O_CLOFORK` flags of `open()` are necessary to avoid a data race in multi-threaded applications.

## SEE ALSO

[`chmod()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/chmod.html), [`close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/close.html), [`creat()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/creat.html), [`dirfd()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dirfd.html), [`dup()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/dup.html), [`exec`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html), [`fcntl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fcntl.html), [`fdopendir()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/fdopendir.html), [`link()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/link.html), [`lseek()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/lseek.html), [`mkdtemp()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mkdtemp.html), [`mknod()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/mknod.html), [`read()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/read.html), [`symlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/symlink.html), [`umask()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/umask.html), [`unlockpt()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/unlockpt.html), [`write()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/write.html)

XBD [11. General Terminal Interface](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap11.html), [`<fcntl.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/fcntl.h.html), [`<sys/stat.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_stat.h.html), [`<sys/types.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/sys_types.h.html)

---

*The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*