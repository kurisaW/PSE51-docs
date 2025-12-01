# pthread_mutexattr_destroy

## 名称

pthread_mutexattr_destroy, pthread_mutexattr_init - 销毁和初始化互斥锁属性对象

## 概要

```c
#include <pthread.h>

int pthread_mutexattr_destroy(pthread_mutexattr_t *attr);
int pthread_mutexattr_init(pthread_mutexattr_t *attr);
```

## 描述

`pthread_mutexattr_destroy()` 函数应销毁一个互斥锁属性对象；该对象实际上变为未初始化状态。实现可能导致 `pthread_mutexattr_destroy()` 将 `attr` 引用的对象设置为无效值。

已销毁的 `attr` 属性对象可以使用 `pthread_mutexattr_init()` 重新初始化；在对象被销毁后其他方式引用该对象的结果是未��义的。

`pthread_mutexattr_init()` 函数应使用实现定义的所有属性的默认值初始化互斥锁属性对象 `attr`。

如果调用 `pthread_mutexattr_init()` 时指定已经初始化的 `attr` 属性对象，则结果未定义。

在互斥锁属性对象已用于初始化一个或多个互斥锁后，任何影响该属性对象的函数（包括销毁）都不应影响任何先前已初始化的互斥锁。

如果传递给 `pthread_mutexattr_destroy()` 的 `attr` 参数值不引用已初始化的互斥锁属性对象，则行为未定义。

## 返回值

成功完成后，`pthread_mutexattr_destroy()` 和 `pthread_mutexattr_init()` 应返回零；否则，应返回错误码以指示错误。

## 错误

`pthread_mutexattr_destroy()` 函数可能失败的情况：

- **EINVAL** - `attr` 指定的值不引用已初始化的互斥锁属性对象。

`pthread_mutexattr_init()` 函数可能失败的情况：

- **ENOMEM** - 内存不足，无法初始化互斥锁属性对象。

这些函数不应返回 [EINTR] 错误码。

## 示例

无。

## 应用用法

如果实现检测到传递给 `pthread_mutexattr_destroy()` 的 `attr` 参数值不引用已初始化的互斥锁属性对象，建议函数应失败并报告 [EINVAL] 错误。

有关属性的一般说明，请参见 `pthread_attr_destroy()`。属性对象允许实现尝试有用的扩展，并允许扩展 POSIX.1-2024 而不更改现有函数。因此，它们提供了 POSIX.1-2024 的未来可扩展性，并减少了过早标准化的诱惑，那些语义尚未被广泛实现或理解。

已讨论的可能附加互斥锁属性的示例包括 `spin_only`、`limited_spin`、`no_spin`、`recursive` 和 `metered`。（解释后两种属性可能意味着什么：递归互斥锁将允许当前所有者多次重新锁定；计量互斥锁将透明地保留队列长度、等待时间等记录。）由于基于共享实现和使用经验对这些属性的有用性尚未达成广泛共识，因此它们尚未在 POSIX.1-2024 中指定。但是，互斥锁属性对象使得可以测试这些概念，以便在以后可能进行标准化。

### 互斥锁属性与性能

已谨慎确保互斥锁属性的默认值被正确定义，使得使用默认值初始化的互斥锁具有足够简单的语义，以便锁定和解锁可以使用等效的测试并设置指令（加上可能的一些其他基本指令）来完成。

如果互斥锁具有非默认属性，至少有一种实现方法可用于减少锁定时测试的成本。实现可以使用的一种方法（并且这可以对完全符合 POSIX 的应用程序完全透明）是秘密预锁定任何初始化为非默认属性的互斥锁。任何稍后尝试锁定此类互斥锁的操作都会导致实现分支到"慢速路径"，就像互斥锁不可用一样；然后，在慢速路径上，实现可以执行"真正的工作"来锁定非默认互斥锁。底层的解锁操作更复杂，因为实现永远不想真正释放这种类型互斥锁的预锁定。这表明，根据硬件的不同，可能存在某些优化，使得无论什么互斥锁属性被认为是"最常用"的，都可以最有效地处理。

### 进程共享内存与同步

POSIX.1-2024 中内存映射函数的存在使得应用程序可能在多个进程访问的内存中分配本节的同步对象（因此，由多个进程的线程访问）。

为了允许这种使用，同时保持通常情况（即在单个进程内使用）高效，定义了 `process-shared` 选项。

如果实现支持 _POSIX_THREAD_PROCESS_SHARED 选项，那么 `process-shared` 属性可用于指示互斥锁或条件变量可能被多个进程的线程访问。

为 `process-shared` 属性选择 PTHREAD_PROCESS_PRIVATE 默认设置，以便默认创建这些同步对象的最有效形式。

使用 PTHREAD_PROCESS_PRIVATE `process-shared` 属性初始化的同步变量只能由初始化它们的进程中的线程操作。使用 PTHREAD_PROCESS_SHARED `process-shared` 属性初始化的同步变量可以由任何有权访问它的进程中的任何线程操作。特别是，这些进程可能存在于初始化进程的生命周期之外。例如，以下代码在映射文件中实现了一个简单的计数信号量，可被许多进程使用。

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
```

```c
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

以下代码用于三个独立的进程，它们在文件 **/tmp/semaphore** 中创建、发送和等待信号量。一旦文件被创建，发送和等待程序即使没有初始化信号量，也会递增和递减计数信号量（根据需要进行等待和唤醒）。

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
```

```c
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
```

```c
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

## 原理

无。

## 未来方向

无。

## 参见

`pthread_attr_destroy()`, `pthread_mutex_destroy()`, `pthread_mutex_init()`

POSIX.1-2024 基本定义卷，`<pthread.h>`

## 变更历史

首次发布于 Issue 5。从 Issue 6 开始包含。

衍生自 IEEE POSIX Pthreads Standard 1003.1c-1995 和 POSIX Threads Extension (1003.1j-2000)。

*信息性文本结束。*