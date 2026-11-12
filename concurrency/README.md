# Java Concurrency – Essential Exam Summary

| Concept | Most important classes / interfaces | Purpose |
|---|---|---|
| Thread pools | `ExecutorService`, `ThreadPoolExecutor` | Manage and execute tasks using reusable threads. |
| Scheduled execution | `ScheduledExecutorService` | Execute tasks after delays or periodically. |
| Parallel processing | `ForkJoinPool` | Execute parallel tasks using work-stealing. |
| Waiting for tasks | `CountDownLatch`, `CyclicBarrier` | Coordinate threads and synchronize execution. |
| Limiting access | `Semaphore` | Limit the number of threads accessing a resource. |
| Explicit locking | `ReentrantLock` | Provide explicit, reentrant locking. |
| Read/write synchronization | `ReentrantReadWriteLock` | Allow multiple readers or one writer. |
| Asynchronous results | `Future`, `CompletableFuture` | Represent and manage asynchronous computations. |
| Thread-safe maps | `ConcurrentHashMap` | Provide concurrent access to a hash map. |
| Thread-safe lists | `CopyOnWriteArrayList` | Provide thread-safe lists optimized for frequent reads. |
| Producer-consumer | `BlockingQueue` | Coordinate producers and consumers using blocking operations. |
| Atomic operations | `AtomicInteger`, `AtomicReference` | Perform thread-safe atomic updates without explicit locks. |

## Important Exam Differences

| Comparison | Key difference |
|---|---|
| `CountDownLatch` vs. `CyclicBarrier` | Latch is one-shot; barrier can be reused. |
| `Runnable` vs. `Callable` | Runnable does not return a result; Callable returns a result and can throw checked exceptions. |
| `Future` vs. `CompletableFuture` | Future retrieves asynchronous results; CompletableFuture also supports chaining and combining operations. |
| `synchronized` vs. `ReentrantLock` | Synchronized uses intrinsic locks; ReentrantLock provides explicit locking features such as `tryLock()` and interruptible acquisition. |
| `ArrayBlockingQueue` vs. `LinkedBlockingQueue` | ArrayBlockingQueue has a fixed capacity; LinkedBlockingQueue can be optionally bounded. |
| `ConcurrentHashMap` vs. `HashMap` | ConcurrentHashMap supports concurrent access; HashMap is not thread-safe. |
| `Semaphore` vs. `Lock` | Semaphore controls access using multiple permits; a lock provides exclusive ownership. |
| `AtomicInteger` vs. `volatile int` | AtomicInteger supports atomic compound operations; volatile only guarantees visibility and ordering, not atomic increments. |

