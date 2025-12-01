# timer_delete

**NAME**

```c
#include <time.h>

int timer_delete(timer_t timerid);
```

**DESCRIPTION**

The `timer_delete()` function deletes the specified timer, `timerid`, previously created by the `timer_create()` function. If the timer is armed when `timer_delete()` is called, the behavior shall be as if the timer is automatically disarmed before removal. The disposition of pending signals for the deleted timer is unspecified.

The behavior is undefined if the value specified by the `timerid` argument to `timer_delete()` does not correspond to a timer ID returned by `timer_create()` but not yet deleted by `timer_delete()`.

**RETURN VALUE**

If successful, the `timer_delete()` function shall return a value of zero. Otherwise, the function shall return a value of -1 and set `errno` to indicate the error.

**ERRORS**

No errors are defined.

---

*The following sections are informative.*

**EXAMPLES**

None.

**APPLICATION USAGE**

None.

**RATIONALE**

If an implementation detects that the value specified by the `timerid` argument to `timer_delete()` does not correspond to a timer ID returned by `timer_create()` but not yet deleted by `timer_delete()`, it is recommended that the function should fail and report an [EINVAL] error.

**FUTURE DIRECTIONS**

None.

**SEE ALSO**

- [`timer_create()`](timer_create.html)
- [`<time.h>`](../basedefs/time.h.html)

**CHANGE HISTORY**

**First released in Issue 5.** Included for alignment with the POSIX Realtime Extension.

**Issue 6**

The `timer_delete()` function is marked as part of the Timers option.

The [ENOSYS] error condition has been removed as stubs need not be provided if an implementation does not support the Timers option.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/139 is applied, updating the ERRORS section so that the [EINVAL] error becomes optional.

**Issue 7**

The `timer_delete()` function is moved from the Timers option to the Base.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0369 [659] is applied.

---

*End of informative text.*

**Copyright © 2001-2024 The IEEE and The Open Group, All Rights Reserved**