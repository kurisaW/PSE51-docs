# atof

[ CX ] The functionality described on this reference page is aligned with the ISO C standard. Any conflict between the requirements described here and the ISO C standard is unintentional. This volume of POSIX.1-2024 defers to the ISO C standard.

## Synopsis

```c
#include <stdlib.h>

double atof(const char *str);
```

## Description

The call `atof(str)` shall be equivalent to:

```c
strtod(str, (char **)NULL),
```

except that the handling of errors may differ. If the value cannot be represented, the behavior is undefined.

## Return Value

The `atof()` function returns the converted value.

## Errors

No errors are defined.

## Examples

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    const char *str = "123.456";
    double result = atof(str);
    printf("Converted value: %f\n", result);
    return 0;
}
```

## Application Usage

The `atof()` function is not required to be thread-safe.

## Rationale

The following sections are informative.

