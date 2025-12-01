# pthread_key_create — 线程特定数据键创建

## 概要 (SYNOPSIS)

```c
#include <pthread.h>

int pthread_key_create(pthread_key_t *key, void (*destructor)(void*));
```

## 描述 (DESCRIPTION)

`pthread_key_create()` 函数应创建一个对进程中所有线程都可见的线程特定数据键。`pthread_key_create()` 提供的键值是不透明对象，用于定位线程特定数据。虽然不同的线程可能使用相同的键值，但由 `pthread_setspecific()` 绑定到键的值是按线程维护的，并在调用线程的生命周期内持续存在。

创建键时，在所有活动线程中，新键应与 NULL 值关联。创建线程时，在新线程中，所有已定义的键应与 NULL 值关联。

可以为每个键值关联一个可选的析构函数。线程退出时，如果键值具有非 NULL 的析构函数指针，且该线程具有与该键关联的非 NULL 值，则该键的值被设置为 NULL，然后调用指向的函数，将先前关联的值作为其唯一参数。当线程退出时如果存在多个析构函数，则析构函数调用的顺序未指定。

如果，在为所有具有关联析构函数的非 NULL 值调用完所有析构函数后，仍然存在一些具有关联析构函数的非 NULL 值，则重复该过程。如果，在对未处理的非 NULL 值进行了至少 {PTHREAD_DESTRUCTOR_ITERATIONS} 次析构函数调用后，仍然存在一些具有关联析构函数的非 NULL 值，实现可以停止调用析构函数，或者可以继续调用析构函数，直到不存在具有关联析构函数的非 NULL 值为止，即使这可能导致无限循环。

## 返回值 (RETURN VALUE)

如果成功，`pthread_key_create()` 函数应将新创建的键值存储在 *key 处并返回零。否则，应返回错误号以指示错误。

## 错误 (ERRORS)

`pthread_key_create()` 函数在以下情况下可能失败：

- **[EAGAIN]**
  - 系统缺乏创建另一个线程特定数据键所需的必要资源，或系统强加的每个进程键总数限制 {PTHREAD_KEYS_MAX} 已被超过。

- **[ENOMEM]**
  - 内存不足，无法创建该键。

`pthread_key_create()` 函数不应返回 [EINTR] 错误码。

## 示例 (EXAMPLES)

以下示例演示了一个函数，在首次调用时初始化线程特定数据键，并将线程特定对象与每个调用线程关联，必要时初始化此对象。

```c
static pthread_key_t key;
static pthread_once_t key_once = PTHREAD_ONCE_INIT;

static void
make_key()
{
    (void) pthread_key_create(&key, NULL);
}

func()
{
    void *ptr;

    (void) pthread_once(&key_once, make_key);
    if ((ptr = pthread_getspecific(key)) == NULL) {
        ptr = malloc(OBJECT_SIZE);
        ...
        (void) pthread_setspecific(key, ptr);
    }
    ...
}
```

注意，键必须在使用 `pthread_getspecific()` 或 `pthread_setspecific()` 之前进行初始化。`pthread_key_create()` 调用可以显式地在模块初始化例程中进行，也可以像本例那样在模块的第一次调用时隐式进行。在键初始化之前使用键的任何尝试都是编程错误，使以下代码不正确。

```c
static pthread_key_t key;

func()
{
    void *ptr;

   /* 键未初始化！！！这不会工作！！！ */
    if ((ptr = pthread_getspecific(key)) == NULL &&
        pthread_setspecific(key, NULL) != 0) {
        pthread_key_create(&key, NULL);
        ...
    }
}
```

## 应用程序用法 (APPLICATION USAGE)

无。

## 基本原理 (RATIONALE)

### 析构函数

通常，为代表特定线程绑定到键的值是指向为调用线程动态分配的存储的指针。与 `pthread_key_create()` 一起指定的析构函数旨在在线程退出时释放此存储。线程取消清理处理程序不能用于此目的，因为线程特定数据可能在取消清理处理程序操作的词法作用域之外持续存在。

如果在线程生命周期期间需要更新与键关联的值，可能需要在新值绑定之前释放与旧值关联的存储。虽然 `pthread_setspecific()` 函数可以自动执行此操作，但此功能不够常用，不足以证明增加的复杂性是合理的。相反，程序员负责释放过期的存储：

```c
old = pthread_getspecific(key);
new = allocate();
destructor(old);
pthread_setspecific(key, new);
```

**注意：**

如果在启用异步取消的情况下运行，上述示例可能会泄漏存储。如果在获取和设置之间没有取消点，则在默认取消状态下不会发生此类问题。

没有析构函数安全函数的概念。如果应用程序不从信号处理程序调用 `pthread_exit()`，或者在调用异步不安全函数时阻止任何可能调用 `pthread_exit()` 的信号处理程序的信号，则可以从析构函数安全地调用所有函数。

### 非幂等数据键创建

曾有请求要求使 `pthread_key_create()` 对于给定的键地址参数是幂等的。这将允许应用程序为给定的键地址多次调用 `pthread_key_create()`，并保证只创建一个键。这样做需要键值之前被初始化（可能在编译时）为已知的空值，并且需要基于键参数的地址和内容执行隐式互斥，以保证只创建一个键。

不幸的是，隐式互斥不仅限于 `pthread_key_create()`。在许多实现上，`pthread_getspecific()` 和 `pthread_setspecific()` 也必须执行隐式互斥，以防止使用未完全存储或尚未可见的键值。这可能会显著增加重要操作的成本，特别是 `pthread_getspecific()`。

因此，这个提案被拒绝了。`pthread_key_create()` 函数不执行隐式同步。程序员有责任确保在使用键之前每个键只调用一次该函数。已经有几种直接的机制可用于完成此操作，包括调用显式模块初始化函数、使用互斥锁和使用 `pthread_once()`。这不会给程序员带来重大负担，不会引入可能令人困惑的 *ad hoc* 隐式同步机制，并且可能允许更常用的线程特定数据操作更高效。

## 未来方向 (FUTURE DIRECTIONS)

无。

## 另请参阅 (SEE ALSO)

- [`pthread_getspecific()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_getspecific.html)
- [`pthread_key_delete()`](https://pubs.opengroup.org/onlinepubs/9799919799/functions/pthread_key_delete.html)
- [`<pthread.h>`](https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/pthread.h.html)

## 变更历史 (CHANGE HISTORY)

### 在第 5 版中首次发布
包含以与 POSIX 线程扩展对齐。

### 第 6 版
`pthread_key_create()` 函数被标记为线程选项的一部分。

应用了 IEEE PASC Interpretation 1003.1c #8，更新了描述部分。

### 第 7 版
`pthread_key_create()` 函数从线程选项移动到基础。

### 第 8 版
应用了 Austin Group Defect 1059，更改了基本原理部分的示例代码。

---

*来源：The Open Group Base Specifications Issue 8, IEEE Std 1003.1-2024*