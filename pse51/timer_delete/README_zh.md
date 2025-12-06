# timer_delete

**名称**

```c
#include <time.h>

int timer_delete(timer_t timerid);
```

**描述**

`timer_delete()` 函数删除指定的定时器 `timerid`，该定时器先前由 `timer_create()` 函数创建。如果在调用 `timer_delete()` 时定时器已启动，则其行为应等同于在删除前自动解除定时器的启动状态。对于已删除定时器的待处理信号的处理方式是未指定的。

如果传递给 `timer_delete()` 的 `timerid` 参数不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的定时器ID，则其行为是未定义的。

**返回值**

如果成功，`timer_delete()` 函数应返回零值。否则，函数应返回 -1 值并设置 `errno` 来指示错误。

**错误**

未定义任何错误。

---

*以下部分为参考资料。*

**示例**

无。

**应用程序用法**

无。

**原理**

如果实现检测到传递给 `timer_delete()` 的 `timerid` 参数值不对应于由 `timer_create()` 返回但尚未被 `timer_delete()` 删除的定时器ID，建议函数应失败并报告 [EINVAL] 错误。

**未来方向**

无。

**参见**

- [`timer_create()`](timer_create.html)
- [`<time.h>`](../basedefs/time.h.html)

**变更历史**

**首次发布于 Issue 5。** 包含在内以与 POSIX 实时扩展对齐。

**Issue 6**

`timer_delete()` 函数被标记为定时器选项的一部分。

如果实现不支持定时器选项，则无需提供存根函数，因此移除了 [ENOSYS] 错误条件。

应用了 IEEE Std 1003.1-2001/Cor 2-2004，项目 XSH/TC2/D6/139，更新了错误部分，使 [EINVAL] 错误变为可选的。

**Issue 7**

`timer_delete()` 函数从定时器选项移动到基础部分。

应用了 POSIX.1-2008，技术勘误表 2，XSH/TC2-2008/0369 [659]。

---

*参考资料结束。*

**版权所有 © 2001-2024 IEEE 和 The Open Group，保留所有权利**
