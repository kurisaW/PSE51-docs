# pthread_mutexattr_init, pthread_mutexattr_destroy - 销毁和初始化互斥锁属性对象

## SYNOPSIS

```c
#include <pthread.h>

int pthread_mutexattr_init(pthread_mutexattr_t *attr);
int pthread_mutexattr_destroy(pthread_mutexattr_t *attr);
```

## DESCRIPTION

`pthread_mutexattr_destroy()` 函数应销毁一个互斥锁属性对象；该对象实际上变为未初始化状态。实现可以使 `pthread_mutexattr_destroy()` 将 `attr` 引用的对象设置为无效值。

已销毁的 `attr` 属性对象可以使用 `pthread_mutexattr_init()` 重新初始化；在对象被销毁后以其他方式引用该对象的结果是未定义的。

`pthread_mutexattr_init()` 函数应使用实现定义的所有属性的默认值来初始化互斥锁属性对象 `attr`。

如果在调用 `pthread_mutexattr_init()` 时指定了一个已经初始化的 `attr` 属性对象，则结果是未定义的。

在互斥锁属性对象已被用于初始化一个或多个互斥锁后，任何影响属性对象的函数（包括销毁）都不应影响任何先前初始化的互斥锁。

如果 `pthread_mutexattr_destroy()` 的 `attr` 参数指定的值不引用已初始化的互斥锁属性对象，则行为是未定义的。

## RETURN VALUE

成功完成后，这些函数应返回零；否则，应返回错误码以指示错误。

## ERRORS

这些函数可能在以下情况下失败：

- **`EINVAL`** - `attr` 指定的值不引用已初始化的互斥锁属性对象。

这些函数不应返回 **`EINTR`** 错误码。

## EXAMPLES

无。

## APPLICATION USAGE

如果实现检测到 `pthread_mutexattr_destroy()` 的 `attr` 参数指定的值不引用已初始化的互斥锁属性对象，建议函数失败并报告 **`[EINVAL]`** 错误。

有关属性的一般说明，请参见 `pthread_attr_destroy()`。属性对象允许实现尝试有用的扩展，并允许在不更改现有函数的情况下扩展 POSIX.1-2024 的这一卷。因此，它们为 POSIX.1-2024 这一卷的未来可扩展性提供了支持，并减少了过早标准化尚未广泛实现或理解的语义的诱惑。

已讨论的可能附加互斥锁属性示例包括 `spin_only`、`limited_spin`、`no_spin`、`recursive` 和 `metered`。（解释后两种属性可能意味着：递归互斥锁将允许当前所有者多次重新加锁；计量互斥锁将透明地保持队列长度、等待时间等记录。）由于基于共享实现和使用经验对这些属性的用处尚未达成广泛一致，它们尚未在 POSIX.1-2024 这一卷中指定。但是，互斥锁属性对象使得可以测试这些概念，以便在以后的时间进行可能的标准化。

### 互斥锁属性与性能

已经谨慎确保互斥锁属性的默认值已定义，使得使用默认值初始化的互斥锁具有足够简单的语义，以便加锁和解锁可以使用测试并设置指令（加上可能的一些其他基本指令）的等效方式完成。

如果互斥锁具有非默认属性，至少有一种实现方法可用于减少锁定时测试的成本。实现可以使用的一种此类方法（并且这对于完全符合 POSIX 的应用程序可以是完全透明的）是秘密地预锁定任何初始化为非默认属性的互斥锁。任何稍后尝试锁定此类互斥锁的操作都会导致实现分支到"慢速路径"，就好像互斥锁不可用一样；然后，在慢速路径上，实现可以执行"真正的工作"来锁定非默认互斥锁。底层解锁操作更复杂，因为实现从不真正想要释放此类互斥锁上的预锁定。这说明，根据硬件的不同，可能存在某些优化可用于以最高效率处理被视为"最常使用"的任何互斥锁属性。

### 进程共享内存与同步

POSIX.1-2024 这一卷中内存映射函数的存在导致应用程序可能可以在多个进程（因此，多个进程的线程）访问的内存中分配本节的同步对象。

为了允许此类使用，同时保持通常情况（即，在单个进程内使用）高效，已定义了**进程共享**选项。

如果实现支持 `_POSIX_THREAD_PROCESS_SHARED` 选项，则**进程共享**属性可用于指示互斥锁或条件变量可由多个进程的线程访问。

已为**进程共享**属性选择 `PTHREAD_PROCESS_PRIVATE` 默认设置，以便默认创建这些同步对象的最有效形式。

使用 `PTHREAD_PROCESS_PRIVATE` **进程共享**属性初始化的同步变量只能由初始化它们的进程中的线程操作。使用 `PTHREAD_PROCESS_SHARED` **进程共享**属性初始化的同步变量可由有权访问它的任何进程中的任何线程操作。特别是，这些进程可能在初始化进程的生命周期之外存在。例如，以下代码在映射文件中实现了一个简单的计数信号量，该信号量可被多个进程使用。

```c
/* sem.h */
struct semaphore {
    pthread_mutex_t lock;
    pthread_cond_t nonzero;
    unsigned count;
};
typedef struct semaphore semaphore_t;

semaphore_t *semaphore_create(char *semaphore_name);
semaphore_t *semaphore_open(char *semaphore_name);
void semaphore_post(semaphore_t *semap);
void semaphore_wait(semaphore_t *semap);
void semaphore_close(semaphore_t *semap);

/* sem.c */
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <pthread.h>
#include "sem.h"

semaphore_t *
semaphore_create(char *semaphore_name)
{
    int fd;
    semaphore_t *semap;
    pthread_mutexattr_t psharedm;
    pthread_condattr_t psharedc;

    fd = open(semaphore_name, O_RDWR | O_CREAT | O_EXCL, 0666);
    if (fd < 0)
        return (NULL);
    (void) ftruncate(fd, sizeof(semaphore_t));
    (void) pthread_mutexattr_init(&psharedm);
    (void) pthread_mutexattr_setpshared(&psharedm,
        PTHREAD_PROCESS_SHARED);
    (void) pthread_condattr_init(&psharedc);
    (void) pthread_condattr_setpshared(&psharedc,
        PTHREAD_PROCESS_SHARED);
    semap = (semaphore_t *) mmap(NULL, sizeof(semaphore_t),
            PROT_READ | PROT_WRITE, MAP_SHARED,
            fd, 0);
    close (fd);
    (void) pthread_mutex_init(&semap->lock, &psharedm);
    (void) pthread_cond_init(&semap->nonzero, &psharedc);
    semap->count = 0;
    return (semap);
}

semaphore_t *
semaphore_open(char *semaphore_name)
{
    int fd;
    semaphore_t *semap;

    fd = open(semaphore_name, O_RDWR, 0666);
    if (fd < 0)
        return (NULL);
    semap = (semaphore_t *) mmap(NULL, sizeof(semaphore_t),
            PROT_READ | PROT_WRITE, MAP_SHARED,
            fd, 0);
    close (fd);
    return (semap);
}

void
semaphore_post(semaphore_t *semap)
{
    pthread_mutex_lock(&semap->lock);
    if (semap->count == 0)
        pthread_cond_signal(&semap->nonzero);
    semap->count++;
    pthread_mutex_unlock(&semap->lock);
}

void
semaphore_wait(semaphore_t *semap)
{
    pthread_mutex_lock(&semap->lock);
    while (semap->count == 0)
        pthread_cond_wait(&semap->nonzero, &semap->lock);
    semap->count--;
    pthread_mutex_unlock(&semap->lock);
}

void
semaphore_close(semaphore_t *semap)
{
    munmap((void *) semap, sizeof(semaphore_t));
}
```

以下代码用于三个独立的进程，这些进程在文件 **/tmp/semaphore** 中创建、发布和等待信号量。一旦文件被创建，发布和等待程序就会递增和递减计数信号量（根据需要等待和唤醒），即使它们没有初始化信号量。

```c
/* create.c */
#include "pthread.h"
#include "sem.h"

int
main(void)
{
    semaphore_t *semap;

    semap = semaphore_create("/tmp/semaphore");
    if (semap == NULL)
        exit(1);
    semaphore_close(semap);
    return (0);
}

/* post.c */
#include "pthread.h"
#include "sem.h"

int
main(void)
{
    semaphore_t *semap;

    semap = semaphore_open("/tmp/semaphore");
    if (semap == NULL)
        exit(1);
    semaphore_post(semap);
    semaphore_close(semap);
    return (0);
}

/* wait.c */
#include "pthread.h"
#include "sem.h"

int
main(void)
{
    semaphore_t *semap;

    semap = semaphore_open("/tmp/semaphore");
    if (semap == NULL)
        exit(1);
    semaphore_wait(semap);
    semaphore_close(semap);
    return (0);
}
```

## RATIONALE

无。

## FUTURE DIRECTIONS

无。

## SEE ALSO

- [`pthread_attr_destroy()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_attr_destroy.html)
- [`pthread_mutex_init()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutex_init.html)
- [`pthread_mutexattr_getpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_getpshared.html)
- [`pthread_mutexattr_setpshared()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_mutexattr_setpshared.html)

POSIX.1-2024 基本定义卷，**\<pthread.h\>**

## COPYRIGHT

本文的部分内容转载并电子复制自 IEEE Std 1003.1-2024，信息技术标准 -- 便携式操作系统接口 (POSIX)，The Open Group 基本规范第 7 版，2024 年版 (IEEE Std 1003.1-2024)。版权所有 © 2024 电气与电子工程师协会和 The Open Group。此版权材料根据 BSD 许可证的条款提供，该许可证可在 https://opensource.org/licenses/BSD-3-Clause 获取。

---
