# pthread_cleanup_pop, pthread_cleanup_push

## SYNOPSIS

```c
#include <pthread.h>

void pthread_cleanup_pop(int execute);
void pthread_cleanup_push(void (*routine)(void*), void *arg);
```

## DESCRIPTION

`pthread_cleanup_pop()` 函数应当从调用线程的取消清理栈顶部移除例程，并可选地调用它（如果 *execute* 为非零值）。

`pthread_cleanup_push()` 函数应当将指定的取消清理处理程序 *routine* 推入调用线程的取消清理栈。当以下情况发生时，取消清理处理程序将从取消清理栈中弹出并使用参数 *arg* 调用：

- 线程退出（即调用 `pthread_exit()`）。
- 线程响应取消请求。
- 线程使用非零的 *execute* 参数调用 `pthread_cleanup_pop()`。

未指定 `pthread_cleanup_push()` 和 `pthread_cleanup_pop()` 是宏还是函数。如果为了访问实际函数而抑制宏定义，或者程序定义了具有这些名称的外部标识符，则行为是未定义的。应用程序应确保它们以语句形式出现，并在相同的词法作用域内成对出现（即，`pthread_cleanup_push()` 宏可以被认为扩展到一个标记列表，其第一个标记是 `'{'`，而 `pthread_cleanup_pop()` 扩展到一个标记列表，其最后一个标记是对应的 `'}'`）。

如果自从填充跳转缓冲区以来有任何未匹配的 `pthread_cleanup_push()` 或 `pthread_cleanup_pop()` 调用，则调用 `longjmp()` 或 `siglongjmp()` 的效果是未定义的。从取消清理处理程序内部调用 `longjmp()` 或 `siglongjmp()` 的效果也是未定义的，除非跳转缓冲区也在取消清理处理程序中填充。

调用取消清理处理程序可能终止任何正在由线程执行的代码块的执行，该代码块的执行在相应的 `pthread_cleanup_push()` 调用之后开始。

使用 `return`、`break`、`continue` 和 `goto` 过早离开由一对 `pthread_cleanup_push()` 和 `pthread_cleanup_pop()` 函数调用描述的代码块的效果是未定义的。

## RETURN VALUE

`pthread_cleanup_push()` 和 `pthread_cleanup_pop()` 函数不应返回值。

## ERRORS

未定义错误。

这些函数不应返回 [EINTR] 错误码。

## EXAMPLES

以下是使用线程原语实现可取消的、写者优先的读写锁的示例：

```c
typedef struct {
    pthread_mutex_t lock;
    pthread_cond_t rcond,
        wcond;
    int lock_count; /* < 0 .. 由写者持有。 */
                    /* > 0 .. 由 lock_count 个读者持有。 */
                    /* = 0 .. 无人持有。 */
    int waiting_writers; /* 等待的写者数量。 */
} rwlock;

void
waiting_reader_cleanup(void *arg)
{
    rwlock *l;

    l = (rwlock *) arg;
    pthread_mutex_unlock(&l->lock);
}

void
lock_for_read(rwlock *l)
{
    pthread_mutex_lock(&l->lock);
    pthread_cleanup_push(waiting_reader_cleanup, l);
    while ((l->lock_count < 0) || (l->waiting_writers != 0))
        pthread_cond_wait(&l->rcond, &l->lock);
    l->lock_count++;
   /*
    * 注意 pthread_cleanup_pop 执行
    * waiting_reader_cleanup。
    */
    pthread_cleanup_pop(1);
}

void
release_read_lock(rwlock *l)
{
    pthread_mutex_lock(&l->lock);
    if (--l->lock_count == 0)
        pthread_cond_signal(&l->wcond);
    pthread_mutex_unlock(&l->lock);
}

void
waiting_writer_cleanup(void *arg)
{
    rwlock *l;

    l = (rwlock *) arg;
    if ((--l->waiting_writers == 0) && (l->lock_count >= 0)) {
       /*
        * 这只在我们被取消时发生。如果锁没有被写者持有，
        * 可能有读者因为 waiting_writers 为正而被阻塞；他们现在可以解除阻塞。
        */
        pthread_cond_broadcast(&l->rcond);
    }
    pthread_mutex_unlock(&l->lock);
}

void
lock_for_write(rwlock *l)
{
    pthread_mutex_lock(&l->lock);
    l->waiting_writers++;
    pthread_cleanup_push(waiting_writer_cleanup, l);
    while (l->lock_count != 0)
        pthread_cond_wait(&l->wcond, &l->lock);
    l->lock_count = -1;
   /*
    * 注意 pthread_cleanup_pop 执行
    * waiting_writer_cleanup。
    */
    pthread_cleanup_pop(1);
}

void
release_write_lock(rwlock *l)
{
    pthread_mutex_lock(&l->lock);
    l->lock_count = 0;
    if (l->waiting_writers == 0)
        pthread_cond_broadcast(&l->rcond);
    else
        pthread_cond_signal(&l->wcond);
    pthread_mutex_unlock(&l->lock);
}

/*
 * 此函数用于初始化读写锁。
 */
void
initialize_rwlock(rwlock *l)
{
    pthread_mutex_init(&l->lock, pthread_mutexattr_default);
    pthread_cond_init(&l->wcond, pthread_condattr_default);
    pthread_cond_init(&l->rcond, pthread_condattr_default);
    l->lock_count = 0;
    l->waiting_writers = 0;
}

reader_thread()
{
    lock_for_read(&lock);
    pthread_cleanup_push(release_read_lock, &lock);
   /*
    * 线程拥有读锁。
    */
    pthread_cleanup_pop(1);
}

writer_thread()
{
    lock_for_write(&lock);
    pthread_cleanup_push(release_write_lock, &lock);
   /*
    * 线程拥有写锁。
    */
pthread_cleanup_pop(1);
}
```

## APPLICATION USAGE

推入和弹出取消清理处理程序的两个例程，`pthread_cleanup_push()` 和 `pthread_cleanup_pop()`，可以被认为是左括号和右括号。它们总是需要匹配。

## RATIONALE

推入和弹出取消清理处理程序的两个例程，`pthread_cleanup_push()` 和 `pthread_cleanup_pop()`，必须出现在相同词法作用域的限制允许高效的宏或编译器实现和高效的存储管理。这些例程作为宏的示例实现可能如下所示：

```c
#define pthread_cleanup_push(rtn,arg) { \
    struct _pthread_handler_rec __cleanup_handler, **__head; \
    __cleanup_handler.rtn = rtn; \
    __cleanup_handler.arg = arg; \
    (void) pthread_getspecific(_pthread_handler_key, &__head); \
    __cleanup_handler.next = *__head; \
    *__head = &__cleanup_handler;

#define pthread_cleanup_pop(ex) \
    *__head = __cleanup_handler.next; \
    if (ex) (*__cleanup_handler.rtn)(__cleanup_handler.arg); \
}
```

这些例程的更有野心的实现可能通过允许编译器注意到取消清理处理程序是常量并且可以内联扩展而做得更好。

POSIX.1-2024 当前版本未指定在 POSIX 系统接口函数中执行的信号处理程序中调用 `longjmp()` 的效果。如果实现想要允许此操作并给程序员合理的行为，`longjmp()` 函数必须调用自调用 `setjmp()` 以来已推入但未弹出的所有取消清理处理程序。

考虑一个使用信号的线程调用的多线程函数。如果在 `qsort()` 操作期间向信号处理程序传递信号，并且该处理程序调用 `longjmp()`（这反过来**不**调用取消清理处理程序），则由 `qsort()` 函数创建的辅助线程不会被取消。相反，它们将继续执行并写入参数数组，即使数组可能已经从栈中弹出。

请注意，指定的清理处理机制特别与 C 语言相关，虽然对表达清理的统一机制的要求是语言无关的，但在其他语言中使用的机制可能完全不同。此外，这种机制实际上只是因为 C 语言缺乏真正的异常机制而必需的，这将是理想的解决方案。

没有取消清理安全函数的概念。如果应用程序在其信号处理程序中没有取消点，在调用异步不安全函数时阻止任何可能包含取消点的信号，或者在调用异步不安全函数时禁用取消，则所有函数都可以安全地从取消清理例程中调用。

## FUTURE DIRECTIONS

无。

## SEE ALSO

`pthread_cancel()`, `pthread_setcancelstate()`

XBD `<pthread.h>`

## CHANGE HISTORY

首次在 Issue 5 中发布。为与 POSIX 线程扩展对齐而包含。

### Issue 6

`pthread_cleanup_pop()` 和 `pthread_cleanup_push()` 函数被标记为线程选项的一部分。

添加了 APPLICATION USAGE 部分。

规范性文本已更新，以避免对应用程序要求使用"必须"一词。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/88，更新了 DESCRIPTION 以描述过早离开由 `pthread_cleanup_push()` 和 `pthread_cleanup_pop()` 函数定义的代码块的后果。

### Issue 7

`pthread_cleanup_pop()` 和 `pthread_cleanup_push()` 函数从线程选项移动到基本（Base）。

应用了 POSIX.1-2008，技术勘误 1，XSH/TC1-2008/0454 [229]。

应用了 POSIX.1-2008，技术勘误 2，XSH/TC2-2008/0268 [624]。

### Issue 8

应用了 Austin Group Defect 613，阐明了自动对象生命周期与取消清理函数的关系。

---
