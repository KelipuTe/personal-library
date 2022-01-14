## 线程

2021-12-20

---

### 并发和并行

当线程数量小于等于cpu核心数时，所有的线程并行执行。cpu的核心够所有线程同时跑，不存在等待的情况。

当线程数量大于cpu核心数时，所有的线程并发执行。cpu的核心不够所有的线程同时跑，所以每个线程执行一个时间片，然后切换到其它线程执行。

### 多线程

详见linux文档：`pthreads(7) - POSIX threads`。

`main()`函数就是主线程。

示例代码：`demo_c/demo_linux_c/thread/thread.c`

### 互斥锁

详见linux文档：`pthread_mutex_lock(3p) - lock and unlock a mutex`。

互斥锁常用于保护临界区的数据。

示例代码：`demo_c/demo_linux_c/thread/mutex_lock.c`

### 条件变量

详见linux文档：`pthread_cond_wait(3p) - wait on a condition`。

条件变量和需要结合互斥锁使用，互斥锁的存在是为了保护条件变量。

示例代码：`demo_c/demo_linux_c/thread/cond_wait.c`