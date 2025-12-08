# atoll

## SYNOPSIS

```c
#include <stdlib.h>

long atol(const char *nptr);
long long atoll(const char *nptr);
```

## DESCRIPTION

[Option Start] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard. [Option End]

Except as noted below, the call `atol(nptr)` shall be equivalent to:

```c
strtol(nptr, (char **)NULL, 10)
```

Except as noted below, the call to `atoll(nptr)` shall be equivalent to:

```c
strtoll(nptr, (char **)NULL, 10)
```

The handling of errors may differ. If the value cannot be represented, the behavior is undefined.

## RETURN VALUE

These functions return the converted value.

## ERRORS

No errors are defined.

## EXAMPLES

None provided.

## APPLICATION USAGE

None provided.

## RATIONALE

None provided.

## FUTURE DIRECTIONS

None provided.

## SEE ALSO

`strtol()`, `strtoll()`

## CHANGE HISTORY

SD5-XSH-ERN-61 is applied, correcting the DESCRIPTION of `atoll()`.

POSIX.1-2008, Technical Corrigendum 2, XSH/TC2-2008/0046 [892] is applied.

