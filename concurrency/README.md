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

# Volatile and Synchronized
volatile is a Java keyword used to ensure visibility of changes to a variable across multiple threads. When a thread modifies a volatile variable, other threads reading that variable will see the most recently written value. It also establishes memory-ordering guarantees (happens-before relationships). However, volatile does not guarantee atomicity for compound operations such as count++, because incrementing involves multiple steps. It is typically used for status flags and variables shared between threads.

synchronized is a Java keyword used to provide mutual exclusion and memory visibility when multiple threads access shared resources. It ensures that only one thread at a time can execute a synchronized block or method protected by the same monitor lock. When a thread releases the lock, its changes become visible to a subsequent thread acquiring that same lock. It prevents race conditions when properly applied to shared mutable state. Unlike volatile, synchronized also provides atomicity for operations performed within the synchronized section.

# Most Important Java Thread Methods

| Method | Description |
|---|---|
| `start()` | Starts a new thread and executes its `run()` method. |
| `run()` | Contains the code executed by the thread. Direct invocation does not start a new thread. |
| `sleep(long millis)` | Pauses the current thread for a specified duration. |
| `join()` | Makes the current thread wait until another thread terminates. |
| `yield()` | Hints to the scheduler that the current thread is willing to give up CPU time. |
| `interrupt()` | Requests interruption of a thread. |
| `isInterrupted()` | Checks whether a thread has been interrupted without clearing the flag. |
| `interrupted()` | Checks and clears the current thread's interrupt status. |
| `isAlive()` | Checks whether a thread has started and has not terminated. |
| `currentThread()` | Returns the currently executing thread. |

## Important Exam Notes

- `start()` vs. `run()`: Only `start()` creates a new thread.
- `sleep()` vs. `wait()`: `sleep()` does not release locks; `wait()` releases the object's monitor.
- `join()`: Waits for another thread to finish.
- `yield()`: Only a scheduling hint; execution order is not guaranteed.
- `interrupt()`: Does not forcibly terminate a thread.
- `wait()`, `notify()` and `notifyAll()` belong to `Object`, not `Thread`.
- `sleep()`, `join()` and `wait()` can throw `InterruptedException`.
