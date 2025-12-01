# sem_clockwait, sem_timedwait — lock a semaphore

## SYNOPSIS

```c
#include <semaphore.h>

int sem_clockwait(sem_t *restrict sem,
                 clockid_t clock_id,
                 const struct timespec *restrict abstime);

int sem_timedwait(sem_t *restrict sem,
                  const struct timespec *restrict abstime);
```

## DESCRIPTION

The `sem_clockwait()` and `sem_timedwait()` functions shall lock the semaphore referenced by `sem` as in the `sem_wait()` function. However, if the semaphore cannot be locked without waiting for another process or thread to unlock the semaphore by performing a `sem_post()` function, this wait shall be terminated when the specified timeout expires.

The timeout shall expire when the absolute time specified by `abstime` passes, as measured by the clock on which timeouts are based (that is, when the value of that clock equals or exceeds `abstime`), or if the absolute time specified by `abstime` has already been passed at the time of the call.

For `sem_timedwait()`, the timeout shall be based on the CLOCK_REALTIME clock. For `sem_clockwait()`, the timeout shall be based on the clock specified by the `clock_id` argument. The resolution of the timeout shall be the resolution of the clock on which it is based. Implementations shall support passing CLOCK_REALTIME and CLOCK_MONOTONIC to `sem_clockwait()` as the `clock_id` argument.

Under no circumstance shall the function fail with a timeout if the semaphore can be locked immediately. The validity of the `abstime` need not be checked if the semaphore can be locked immediately.

## RETURN VALUE

The `sem_clockwait()` and `sem_timedwait()` functions shall return zero if the calling process successfully performed the semaphore lock operation on the semaphore designated by `sem`. If the call was unsuccessful, the state of the semaphore shall be unchanged, and the functions shall return a value of -1 and set `errno` to indicate the error.

## ERRORS

The `sem_clockwait()` and `sem_timedwait()` functions shall fail if:

- **[EINVAL]**
  - The process or thread would have blocked, and either the `abstime` parameter specified a nanoseconds field value less than zero or greater than or equal to 1000 million, or the `sem_clockwait()` function was passed an invalid or unsupported `clock_id` value.

- **[ETIMEDOUT]**
  - The semaphore could not be locked before the specified timeout expired.

The `sem_clockwait()` and `sem_timedwait()` functions may fail if:

- **[EDEADLK]**
  - A deadlock condition was detected.

- **[EINTR]**
  - A signal interrupted the function.

- **[EINVAL]**
  - The `sem` argument does not refer to a valid semaphore.

## EXAMPLES

The program shown below operates on an unnamed semaphore. The program expects two command-line arguments. The first argument specifies a seconds value that is used to set an alarm timer to generate a SIGALRM signal. This handler performs a `sem_post()` to increment the semaphore that is being waited on in `main()` using `sem_clockwait()`. The second command-line argument specifies the length of the timeout, in seconds, for `sem_clockwait()`.

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <semaphore.h>
#include <time.h>
#include <assert.h>
#include <errno.h>
#include <signal.h>

sem_t sem;

static void
handler(int sig)
{
    int sav_errno = errno;
    static const char info_msg[] = "sem_post() from handler\n";
    write(STDOUT_FILENO, info_msg, sizeof info_msg - 1);
    if (sem_post(&sem) == -1) {
        static const char err_msg[] = "sem_post() failed\n";
        write(STDERR_FILENO, err_msg, sizeof err_msg - 1);
        _exit(EXIT_FAILURE);
    }
    errno = sav_errno;
}

int
main(int argc, char *argv[])
{
    struct sigaction sa;
    struct timespec ts;
    int s;

    if (argc != 3) {
        fprintf(stderr, "Usage: %s <alarm-secs> <wait-secs>\n",
            argv[0]);
        exit(EXIT_FAILURE);
    }

    if (sem_init(&sem, 0, 0) == -1) {
        perror("sem_init");
        exit(EXIT_FAILURE);
    }

    /* Establish SIGALRM handler; set alarm timer using argv[1] */

    sa.sa_handler = handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;
    if (sigaction(SIGALRM, &sa, NULL) == -1) {
        perror("sigaction");
        exit(EXIT_FAILURE);
    }

    alarm(atoi(argv[1]));

    /* Calculate relative interval as current time plus
       number of seconds given argv[2] */

    if (clock_gettime(CLOCK_MONOTONIC, &ts) == -1) {
        perror("clock_gettime");
        exit(EXIT_FAILURE);
    }
    ts.tv_sec += atoi(argv[2]);

    printf("main() about to call sem_clockwait()\n");
    while ((s = sem_clockwait(&sem, CLOCK_MONOTONIC, &ts)) == -1 &&
           errno == EINTR)
        continue;       /* Restart if interrupted by handler */

    /* Check what happened */

    if (s == -1) {
        if (errno == ETIMEDOUT)
            printf("sem_clockwait() timed out\n");
        else
            perror("sem_clockwait");
    } else
        printf("sem_clockwait() succeeded\n");

    exit((s == 0) ? EXIT_SUCCESS : EXIT_FAILURE);
}
```

## APPLICATION USAGE

Applications using these functions may be subject to priority inversion, as discussed in XBD 3.275 Priority Inversion.

## RATIONALE

None.

## FUTURE DIRECTIONS

None.

## SEE ALSO

- [`sem_post()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_post.html)
- [`sem_trywait()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/sem_trywait.html)
- [`semctl()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semctl.html)
- [`semget()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semget.html)
- [`semop()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/semop.html)
- [`time()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/time.html)

XBD 3.275 Priority Inversion, `<semaphore.h>`, `<time.h>`

## CHANGE HISTORY

### First released in Issue 6. Derived from IEEE Std 1003.1d-1999.

IEEE Std 1003.1-2001/Cor 2-2004, item XSH/TC2/D6/120 is applied, updating the ERRORS section so that the [EINVAL] error becomes optional.

### Issue 7

The `sem_timedwait()` function is moved from the Semaphores option to the Base.

Functionality relating to the Timers option is moved to the Base.

An example is added.

POSIX.1-2008, Technical Corrigendum 1, XSH/TC1-2008/0529 [138] is applied.

### Issue 8

Austin Group Defect 592 is applied, removing text relating to `<time.h>` from the SYNOPSIS and DESCRIPTION sections.

Austin Group Defect 1216 is applied, adding `sem_clockwait()`.

---

*The Open Group Base Specifications Issue 8*
*IEEE Std 1003.1-2024*
*Copyright © 2001-2024 The IEEE and The Open Group*