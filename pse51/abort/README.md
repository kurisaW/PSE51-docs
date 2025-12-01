# abort — generate an abnormal process abort

## SYNOPSIS

```c
#include <stdlib.h>

_Noreturn void abort(void);
```

## DESCRIPTION

The `abort()` function shall cause abnormal process termination to occur, unless a SIGABRT signal that it generates is caught and the signal handler does not return.

The abnormal termination processing shall include the default actions defined for SIGABRT and may include an attempt to effect `fclose()` on all open streams.

The SIGABRT signal shall be sent to the calling thread as if by means of `raise()` with the argument SIGABRT. If this signal does not terminate the process (for example, if the signal is caught and the handler returns), `abort()` may change the disposition of SIGABRT to SIG_DFL and send the signal (in the same way) again. If a second signal is sent and it does not terminate the process, the behavior is unspecified, except that the `abort()` call shall not return.

The status made available to `wait()`, `waitid()`, or `waitpid()` by `abort()` shall be that of a process terminated by the SIGABRT signal. The `abort()` function shall override blocking or ignoring the SIGABRT signal.

## RETURN VALUE

The `abort()` function shall not return.

## ERRORS

No errors are defined.

## EXAMPLES

None.

## APPLICATION USAGE

Catching the signal is intended to provide the application developer with a portable means to abort processing, free from possible interference from any implementation-supplied functions.

## RATIONALE

Historically, `abort()` has been implemented by calling other signal manipulation functions such as `raise()`, `sigaction()`, and `pthread_sigmask()`. This means that its operation can be affected by concurrent actions in other threads. For example, if `abort()` attempts to terminate the process by calling `sigaction()` to change the disposition for SIGABRT to SIG_DFL and then calling `raise()`, another thread could change the disposition in between those two calls, resulting in the process not being terminated. If this happens, the only requirement is that `abort()` does not return. An implementation could call those functions in a loop (which could in theory then execute indefinitely), or could terminate the process by calling `_exit()` (which would ensure termination but result in the wrong wait status). To avoid these issues, implementations are encouraged to implement `abort()` in a manner such that its operation cannot be affected by concurrent actions in other threads. For example, it could first halt the execution of all other threads, or it could terminate the process using a "terminate as if by a signal" system call instead of by raising (a second) SIGABRT.

The ISO/IEC 9899:1999 standard required (and the current standard still requires) the `abort()` function to be async-signal-safe. Since POSIX.1-2024 defers to the ISO C standard, this required a change to the DESCRIPTION from "shall include the effect of `fclose()`" to "may include an attempt to effect `fclose()`."

The revised wording permits some backwards-compatibility and avoids a potential deadlock situation.

The Open Group Base Resolution bwg2002-003 is applied, removing the following XSI shaded paragraph from the DESCRIPTION:

"On XSI-conformant systems, in addition the abnormal termination processing shall include the effect of `fclose()` on message catalog descriptors."

There were several reasons to remove this paragraph:

* No special processing of open message catalogs needs to be performed prior to abnormal process termination.
* The main reason to specifically mention that `abort()` includes the effect of `fclose()` on open streams is to flush output queued on the stream. Message catalogs in this context are read-only and, therefore, do not need to be flushed.
* The effect of `fclose()` on a message catalog descriptor is unspecified. Message catalog descriptors are allowed, but not required to be implemented using a file descriptor, but there is no mention in POSIX.1-2024 of a message catalog descriptor using a standard I/O stream FILE object as would be expected by `fclose()`.

## FUTURE DIRECTIONS

A future version of this standard may require `abort()` to be implemented in a manner such that its operation cannot be affected by concurrent actions in other threads.

## SEE ALSO

`_exit()`, `kill()`, `raise()`, `signal()`, `wait()`, `waitid()`

XBD `<stdlib.h>`

## CHANGE HISTORY

First released in Issue 1. Derived from Issue 1 of the SVID.

### Issue 6

Extensions beyond the ISO C standard are marked.

Changes are made to the DESCRIPTION for alignment with the ISO/IEC 9899:1999 standard.

The Open Group Base Resolution bwg2002-003 is applied.

IEEE Std 1003.1-2001/Cor 1-2002, item XSH/TC1/D6/10 is applied, changing the DESCRIPTION of abnormal termination processing and adding to the RATIONALE section.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/9 is applied, changing "implementation-defined functions" to "implementation-supplied functions" in the APPLICATION USAGE section.

### Issue 8

Austin Group Defect 906 is applied, clarifying how the behavior of `abort()` may be affected by concurrent actions in other threads.

Austin Group Defect 1302 is applied, aligning this function with the ISO/IEC 9899:2018 standard.