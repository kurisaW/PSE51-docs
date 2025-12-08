# freopen

## SYNOPSIS

```c
#include <stdio.h>

FILE *freopen(const char *restrict pathname,
              const char *restrict mode,
              FILE *restrict stream);
```

## DESCRIPTION

[Option Start] Except for the "exclusive access" requirement (see fopen()), the functionality described on this reference page is aligned with the ISO C standard. Any other conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard for all freopen() functionality except in relation to "exclusive access". [Option End]

The freopen() function shall first attempt to flush the stream associated with stream as if by a call to fflush(stream). Failure to flush the stream successfully shall be ignored. If pathname is not a null pointer, freopen() shall close any file descriptor associated with stream. Failure to close the file descriptor successfully shall be ignored. The error and end-of-file indicators for the stream shall be cleared.

The freopen() function shall open the file whose pathname is the string pointed to by pathname and associate the stream pointed to by stream with it. The mode argument shall be used just as in fopen().

The original stream shall be closed regardless of whether the subsequent open succeeds.

If pathname is a null pointer, the freopen() function shall attempt to change the mode of the stream to that specified by mode, as if the name of the file currently associated with the stream had been used. In this case, the file descriptor associated with the stream need not be closed if the call to freopen() succeeds. It is implementation-defined which changes of mode are permitted (if any), and under what circumstances.

After a successful call to the freopen() function, the orientation of the stream shall be cleared, [Option Start] the encoding rule shall be cleared, [Option End] and the associated mbstate_t object shall be set to describe an initial conversion state.

[Option Start] If pathname is not a null pointer, or if pathname is a null pointer and the specified mode change necessitates the file descriptor associated with the stream to be closed and reopened, the file descriptor associated with the reopened stream shall be allocated and opened as if by a call to open() with the flags specified for fopen() with the same mode argument. [Option End]

## RETURN VALUE

Upon successful completion, freopen() shall return the value of stream. Otherwise, a null pointer shall be returned, [Option Start] and errno shall be set to indicate the error. [Option End]

## ERRORS

The freopen() function shall fail if:

- **[EACCES]**
  [Option Start] Search permission is denied on a component of the path prefix, or the file exists and the permissions specified by mode are denied, or the file does not exist and write permission is denied for the parent directory of the file to be created. [Option End]

- **[EBADF]**
  [Option Start] The file descriptor underlying the stream is not a valid file descriptor when pathname is a null pointer. [Option End]

- **[EILSEQ]**
  [Option Start] The mode argument begins with w or a, the file did not previously exist, and the last pathname component is not a portable filename and cannot be created in the target directory. [Option End]

- **[EEXIST]**
  [Option Start] The mode argument begins with w or a and includes x, but the file already exists. [Option End]

- **[EINTR]**
  [Option Start] A signal was caught during freopen(). [Option End]

- **[EISDIR]**
  [Option Start] The named file is a directory and mode requires write access. [Option End]

- **[ELOOP]**
  [Option Start] A loop exists in symbolic links encountered during resolution of the pathname argument. [Option End]

- **[EMFILE]**
  [Option Start] All file descriptors available to the process are currently open. [Option End]

- **[ENAMETOOLONG]**
  [Option Start] The length of a component of a pathname is longer than {NAME_MAX}. [Option End]

- **[ENFILE]**
  [Option Start] The maximum allowable number of files is currently open in the system. [Option End]

- **[ENOENT]**
  [Option Start] The mode string begins with 'r' and a component of pathname does not name an existing file, or mode begins with 'w' or 'a' and a component of the path prefix of pathname does not name an existing file, or pathname is an empty string. [Option End]

- **[ENOENT] or [ENOTDIR]**
  [Option Start] The pathname argument contains at least one non-<slash> character and ends with one or more trailing <slash> characters. If pathname without the trailing <slash> characters would name an existing file, an [ENOENT] error shall not occur. [Option End]

- **[ENOSPC]**
  [Option Start] The directory or file system that would contain the new file cannot be expanded, the file does not exist, and it was to be created. [Option End]

- **[ENOTDIR]**
  [Option Start] A component of the path prefix names an existing file that is neither a directory nor a symbolic link to a directory, or the pathname argument contains at least one non-<slash> character and ends with one or more trailing <slash> characters and the last pathname component names an existing file that is neither a directory nor a symbolic link to a directory. [Option End]

- **[ENXIO]**
  [Option Start] The named file is a character special or block special file, and the device associated with this special file does not exist. [Option End]

- **[EOVERFLOW]**
  [Option Start] The named file is a regular file and the size of the file cannot be represented correctly in an object of type off_t. [Option End]

- **[EROFS]**
  [Option Start] The named file resides on a read-only file system and mode requires write access. [Option End]

The freopen() function may fail if:

- **[EBADF]**
  [Option Start] The mode with which the file descriptor underlying the stream was opened does not support the requested mode when pathname is a null pointer. [Option End]

- **[EINVAL]**
  [Option Start] The value of the mode argument is not valid. [Option End]

- **[ELOOP]**
  [Option Start] More than {SYMLOOP_MAX} symbolic links were encountered during resolution of the pathname argument. [Option End]

- **[ENAMETOOLONG]**
  [Option Start] The length of a pathname exceeds {PATH_MAX}, or pathname resolution of a symbolic link produced an intermediate result with a length that exceeds {PATH_MAX}. [Option End]

- **[ENOMEM]**
  [Option Start] Insufficient storage space is available. [Option End]

- **[ENXIO]**
  [Option Start] A request was made of a nonexistent device, or the request was outside the capabilities of the device. [Option End]

- **[ETXTBSY]**
  [Option Start] The file is a pure procedure (shared text) file that is being executed and mode requires write access. [Option End]

---

*The following sections are informative.*

## EXAMPLES

### Directing Standard Output to a File

The following example logs all standard output to the /tmp/logfile file.

```c
#include <stdio.h>
...
FILE *fp;
...
fp = freopen ("/tmp/logfile", "a+", stdout);
...
```

## APPLICATION USAGE

The freopen() function is typically used to attach the pre-opened streams associated with stdin, stdout, and stderr to other files.

Since implementations are not required to support any stream mode changes when the pathname argument is NULL, portable applications cannot rely on the use of freopen() to change the stream mode, and use of this feature is discouraged. The feature was originally added to the ISO C standard in order to facilitate changing stdin and stdout to binary mode. Since a 'b' character in the mode has no effect on POSIX systems, this use of the feature is unnecessary in POSIX applications. However, even though the 'b' is ignored, a successful call to freopen(NULL, "wb", stdout) does have an effect. In particular, for regular files it truncates the file and sets the file-position indicator for the stream to the start of the file. It is possible that these side-effects are an unintended consequence of the way the feature was specified in the ISO/IEC 9899:1999 standard (and still is in the current standard), but unless or until the ISO C standard is changed, applications which successfully call freopen(NULL, "wb", stdout) will behave in unexpected ways on conforming systems in situations such as:

```
{ appl file1; appl file2; } > file3
```

which will result in file3 containing only the output from the second invocation of appl.

See also the APPLICATION USAGE for fopen().

## RATIONALE

See the RATIONALE for fopen().

## FUTURE DIRECTIONS

None.

## SEE ALSO

- 2.5 Standard I/O Streams
- fclose()
- fdopen()
- fflush()
- fmemopen()
- fopen()
- mbsinit()
- open()
- open_memstream()
- <stdio.h>

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 5

The DESCRIPTION is updated to indicate that the orientation of the stream is cleared and the conversion state of the stream is set to an initial conversion state by a successful call to the freopen() function.

Large File Summit extensions are added.

### Issue 6

Extensions beyond the ISO C standard are marked.

The following new requirements on POSIX implementations derive from alignment with the Single UNIX Specification:

- In the DESCRIPTION, text is added to indicate setting of the offset maximum in the open file description. This change is to support large files.
- In the ERRORS section, the [EOVERFLOW] condition is added. This change is to support large files.
- The [ELOOP] mandatory error condition is added.
- A second [ENAMETOOLONG] is added as an optional error condition.
- The [EINVAL], [ENOMEM], [ENXIO], and [ETXTBSY] optional error conditions are added.

The following changes are made for alignment with the ISO/IEC 9899:1999 standard:

- The freopen() prototype is updated.
- The DESCRIPTION is updated.

The wording of the mandatory [ELOOP] error condition is updated, and a second optional [ELOOP] error condition is added.

The DESCRIPTION is updated regarding failure to close, changing the "file" to "file descriptor".

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/40 is applied, adding the following sentence to the DESCRIPTION: "In this case, the file descriptor associated with the stream need not be closed if the call to freopen() succeeds.".

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/41 is applied, adding an mandatory [EBADF] error, and an optional [EBADF] error to the ERRORS section.

### Issue 7

Austin Group Interpretation 1003.1-2001 #043 is applied, clarifying that the freopen() function allocates a file descriptor as per open().

Austin Group Interpretation 1003.1-2001 #143 is applied.

Austin Group Interpretation 1003.1-2001 #159 is applied, clarifying requirements for the flags set on the open file description.

SD5-XBD-ERN-4 is applied, changing the definition of the [EMFILE] error.

SD5-XSH-ERN-150 and SD5-XSH-ERN-219 are applied.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0181 [291,433], XSH/TC1-2008/0182 [146,433], XSH/TC1-2008/0183 [324], and XSH/TC1-2008/0184 [14] are applied.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0134 [822] is applied.

### Issue 8

Austin Group Defect 293 is applied, adding the [EILSEQ] error.

Austin Group Defect 411 is applied, adding the e and x mode string characters.

Austin Group Defect 1200 is applied, correcting the argument name in the [ELOOP] errors.

Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.

