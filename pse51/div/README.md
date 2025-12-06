# div

## Synopsis

```c
#include <stdlib.h>

div_t div(int numer, int denom);
```

## Description

The `div()` function shall compute the quotient and remainder of the division of the numerator `numer` by the denominator `denom`. If the division is inexact, the resulting quotient is the integer of lesser magnitude that is the nearest to the algebraic quotient. If the result cannot be represented, the behavior is undefined; otherwise, `quot*denom+rem` shall equal `numer`.

## Return Value

The `div()` function shall return a structure of type `div_t`, comprising both the quotient and the remainder. The structure includes the following members, in any order:

```c
int  quot;  /* quotient */
int  rem;   /* remainder */
```

## Examples

None provided.

## Application Usage

None provided.

## Rationale

None provided.

## Future Directions

None provided.

## See Also

None provided.

## Copyright

The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.