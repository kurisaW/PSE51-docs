# munmap — unmap pages of memory

## SYNOPSIS

```c
#include <sys/mman.h>

int munmap(void *addr, size_t len);
```

## DESCRIPTION

The `munmap()` function shall remove any mappings for those entire pages containing any part of the address space of the process starting at `addr` and continuing for `len` bytes. Further references to these pages shall result in the generation of a SIGSEGV signal to the process. If there are no mappings in the specified address range, then `munmap()` has no effect.

The implementation may require that `addr` be a multiple of the page size as returned by `sysconf()`.

If a mapping to be removed was private, any modifications made in this address range shall be discarded.

[ML|MLR] Any memory locks (see `mlock()` and `mlockall()`) associated with this address range shall be removed, as if by an appropriate call to `munlock()`.

[TYM] If a mapping removed from a typed memory object causes the corresponding address range of the memory pool to be inaccessible by any process in the system except through allocatable mappings (that is, mappings of typed memory objects opened with the POSIX_TYPED_MEM_MAP_ALLOCATABLE flag), then that range of the memory pool shall become deallocated and may become available to satisfy future typed memory allocation requests.

A mapping removed from a typed memory object opened with the POSIX_TYPED_MEM_MAP_ALLOCATABLE flag shall not affect in any way the availability of that typed memory for allocation.

The behavior of this function is unspecified if the mapping was not established by a call to `mmap()`.

## RETURN VALUE

Upon successful completion, `munmap()` shall return 0; otherwise, it shall return -1 and set `errno` to indicate the error.

## ERRORS

The `munmap()` function shall fail if:

- **[EINVAL]** Addresses in the range [`addr`,`addr`+`len`) are outside the valid range for the address space of a process.
- **[EINVAL]** The `len` argument is 0.

The `munmap()` function may fail if:

- **[EINVAL]** The `addr` argument is not a multiple of the page size as returned by `sysconf()`.

## EXAMPLES

None.

## APPLICATION USAGE

None.

## RATIONALE

The `munmap()` function corresponds to SVR4, just as the `mmap()` function does.

It is possible that an application has applied process memory locking to a region that contains shared memory. If this has occurred, the `munmap()` call ignores those locks and, if necessary, causes those locks to be removed.

Most implementations require that `addr` is a multiple of the page size as returned by `sysconf()`.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- `mlock()`
- `mlockall()`
- `mmap()`
- `posix_typed_mem_open()`
- `sysconf()`
- XBD `<sys/mman.h>`

## CHANGE HISTORY

First released in Issue 4, Version 2.

### Issue 5

Moved from X/OPEN UNIX extension to BASE.

Aligned with `munmap()` in the POSIX Realtime Extension as follows:

- The DESCRIPTION is extensively reworded.
- The SIGBUS error is no longer permitted to be generated.

### Issue 6

The `munmap()` function is marked as part of the Memory Mapped Files and Shared Memory Objects option.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- The DESCRIPTION is updated to state that implementations require `addr` to be a multiple of the page size.
- The [EINVAL] error conditions are added.

The following changes are made for alignment with IEEE Std 1003.1j-2000:

- Semantics for typed memory objects are added to the DESCRIPTION.
- The `posix_typed_mem_open()` function is added to the SEE ALSO section.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/36 is applied, changing the margin code in the SYNOPSIS from MF|SHM to MC3 (notation for MF|SHM|TYM).

### Issue 7

Austin Group Interpretation 1003.1-2001 #078 is applied, clarifying page alignment requirements.

The `munmap()` function is moved from the Memory Mapped Files option to the Base.

---
