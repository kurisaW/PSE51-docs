# sem_open — initialize and open a named semaphore

## SYNOPSIS

```c
#include <semaphore.h>

sem_t *sem_open(const char *name, int oflag, ...);
```

## DESCRIPTION

The `sem_open()` function shall establish a connection between a named semaphore and a process. A named semaphore may be implemented using a file descriptor. Following a call to `sem_open()` with semaphore name `name`, the process may reference the semaphore associated with `name` using the semaphore handle returned from the call. This semaphore can be used in subsequent calls to [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html), [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html), [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html), [`sem_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html), [`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html), and [`sem_wait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_wait.html). The semaphore remains usable by this process until the semaphore is closed by a successful call to [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html), [`_exit()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/_exit.html), or one of the [exec](https://pubs.opengroup.org/onlinepubs/9799919799/functions/exec.html) functions.

The `oflag` argument controls whether the semaphore is created or merely accessed by the call to `sem_open()`. The following flag bits may be set in `oflag`:

### O_CREAT

This flag is used to create a semaphore if it does not already exist. If O_CREAT is set and the semaphore already exists, then O_CREAT has no effect, except as noted under O_EXCL. Otherwise, `sem_open()` creates a named semaphore. The O_CREAT flag requires a third and a fourth argument: `mode`, which is of type `mode_t`, and `value`, which is of type `unsigned`. The semaphore is created with an initial value of `value`. Valid initial values for semaphores are less than or equal to {SEM_VALUE_MAX}.

The user ID of the semaphore shall be set to the effective user ID of the process. The group ID of the semaphore shall be set to the effective group ID of the process; however, if the `name` argument is visible in the file system, the group ID may be set to the group ID of the containing directory. The permission bits of the semaphore are set to the value of the `mode` argument except those set in the file mode creation mask of the process. When bits in `mode` other than the file permission bits are specified, the effect is unspecified.

After the semaphore named `name` has been created by `sem_open()` with the O_CREAT flag, other processes can connect to the semaphore by calling `sem_open()` with the same value of `name`.

### O_EXCL

If O_EXCL and O_CREAT are set, `sem_open()` fails if the semaphore `name` exists. The check for the existence of the semaphore and the creation of the semaphore if it does not exist are atomic with respect to other processes executing `sem_open()` with O_EXCL and O_CREAT set. If O_EXCL is set and O_CREAT is not set, the effect is undefined.

If flags other than O_CREAT and O_EXCL are specified in the `oflag` parameter, the effect is unspecified.

The `name` argument points to a string naming a semaphore object. It is unspecified whether the name appears in the file system and is visible to functions that take pathnames as arguments. The `name` argument conforms to the construction rules for a pathname, except that the interpretation of `<slash>` characters other than the leading `<slash>` character in `name` is implementation-defined, and that the length limits for the `name` argument are implementation-defined and need not be the same as the pathname limits {PATH_MAX} and {NAME_MAX}. If `name` begins with the `<slash>` character, then processes calling `sem_open()` with the same value of `name` shall refer to the same semaphore object, as long as that name has not been removed. If `name` does not begin with the `<slash>` character, the effect is implementation-defined.

If a process makes multiple successful calls to `sem_open()` with the same value for `name`, there have been no intervening calls to [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html) for `name`, and at least one open handle for this semaphore has not been closed with a [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html) call, it is implementation-defined whether the same handle or a unique handle is returned for each such successful call.

References to copies of the semaphore produce undefined results.

## RETURN VALUE

Upon successful completion, the `sem_open()` function shall return the address of the semaphore. Otherwise, it shall return a value of SEM_FAILED and set `errno` to indicate the error. The symbol SEM_FAILED is defined in the [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html) header. No successful return from `sem_open()` shall return the value SEM_FAILED.

## ERRORS

If any of the following conditions occur, the `sem_open()` function shall return SEM_FAILED and set `errno` to the corresponding value:

- **[EACCES]** The named semaphore exists and the permissions specified by `oflag` are denied, or the named semaphore does not exist and permission to create the named semaphore is denied.
- **[EEXIST]** O_CREAT and O_EXCL are set and the named semaphore already exists.
- **[EINTR]** The `sem_open()` operation was interrupted by a signal.
- **[EINVAL]** The `sem_open()` operation is not supported for the given name, or O_CREAT was specified in `oflag` and `value` was greater than {SEM_VALUE_MAX}.
- **[ENOENT]** O_CREAT is not set and the named semaphore does not exist.
- **[ENOMEM]** There is insufficient memory for the creation of the new named semaphore.
- **[ENOSPC]** There is insufficient space on a storage device for the creation of the new named semaphore.

If any of the following conditions occur, the `sem_open()` function may return SEM_FAILED and set `errno` to the corresponding value:

- **[EMFILE]** Too many semaphore descriptors or file descriptors are currently in use by this process.
- **[ENAMETOOLONG]** The length of the `name` argument exceeds {_POSIX_PATH_MAX} on systems that do not support the XSI option [XSI] or exceeds {_XOPEN_PATH_MAX} on XSI systems, or has a pathname component that is longer than {_POSIX_NAME_MAX} on systems that do not support the XSI option [XSI] or longer than {_XOPEN_NAME_MAX} on XSI systems.
- **[ENFILE]** Too many semaphore descriptors or file descriptors are currently open in the system.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

Early drafts required an error return value of -1 with the type `sem_t *` for the `sem_open()` function, which is not guaranteed to be portable across implementations. The revised text provides the symbolic error code SEM_FAILED to eliminate the type conflict.

## FUTURE DIRECTIONS

A future version might require the `sem_open()` and [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html) functions to have semantics similar to normal file system operations.

## SEE ALSO

[`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html), [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html), [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html), [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html), [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html), [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html), [`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html), [`sem_unlink()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_unlink.html)

XBD [`<semaphore.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/semaphore.h.html)

## CHANGE HISTORY

First released in Issue 5. Included for alignment with the POSIX Realtime Extension.

### Issue 6

The `sem_open()` function is marked as part of the Semaphores option.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Semaphores option.

The [`sem_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html) function is added to the SEE ALSO section for alignment with IEEE Std 1003.1d-1999.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/117 is applied, updating the DESCRIPTION to add the [`sem_timedwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_timedwait.html) function for alignment with IEEE Std 1003.1d-1999.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/118 is applied, updating the DESCRIPTION to describe the conditions to return the same semaphore address on a call to `sem_open()`. The words "and at least one previous successful `sem_open()` call for this semaphore has not been matched with a [`sem_close()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_close.html) call" are added.

### Issue 7

Austin Group Interpretation 1003.1-2001 #066 is applied, updating the [ENOSPC] error case and adding the [ENOMEM] error case.

Austin Group Interpretation 1003.1-2001 #077 is applied, clarifying the `name` argument and adding [ENAMETOOLONG] as a "may fail" error.

Austin Group Interpretation 1003.1-2001 #141 is applied, adding FUTURE DIRECTIONS.

SD5-XSH-ERN-170 is applied, updating the DESCRIPTION to clarify the wording for setting the user ID and group ID of the semaphore.

The `sem_open()` function is moved from the Semaphores option to the Base.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0527 [37] is applied.

### Issue 8

Austin Group Defect 368 is applied, adding a statement that a named semaphore may be implemented using a file descriptor and changing the ERRORS section.

Austin Group Defect 1216 is applied, adding [`sem_clockwait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_clockwait.html).

Austin Group Defect 1324 is applied, making it implementation-defined whether the same handle or a unique handle is returned when multiple successful calls to `sem_open()` are made with the same value for `name`.