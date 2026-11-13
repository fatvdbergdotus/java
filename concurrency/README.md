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

'''java
import java.util.concurrent.*;
import java.util.concurrent.locks.*;
import java.util.concurrent.ThreadLocalRandom;

public class SynchronizationAids {

    // 1. CountDownLatch
    // Wait until all worker threads have completed.
    static void countDownLatchExample() throws InterruptedException {
        System.out.println("\n--- CountDownLatch ---");

        CountDownLatch latch = new CountDownLatch(3);

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            new Thread(() -> {
                System.out.println("Worker " + id + " started");

                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    return;
                }

                System.out.println("Worker " + id + " finished");
                latch.countDown();
            }).start();
        }

        latch.await();
        System.out.println("All workers completed!");
    }


    // 2. CyclicBarrier
    // All threads wait until they reach the same barrier.
    static void cyclicBarrierExample() {
        System.out.println("\n--- CyclicBarrier ---");

        CyclicBarrier barrier = new CyclicBarrier(3,
                () -> System.out.println("All workers reached the barrier!")
        );

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            new Thread(() -> {
                try {
                    System.out.println("Worker " + id + " preparing");

                    Thread.sleep(500);

                    barrier.await();

                    System.out.println("Worker " + id + " continuing");

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }


    // 3. Exchanger<V>
    // Two threads exchange objects.
    static void exchangerExample() {
        System.out.println("\n--- Exchanger ---");

        Exchanger<String> exchanger = new Exchanger<>();

        Thread t1 = new Thread(() -> {
            try {
                String received = exchanger.exchange("Message A");
                System.out.println("Thread 1 received: " + received);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        Thread t2 = new Thread(() -> {
            try {
                String received = exchanger.exchange("Message B");
                System.out.println("Thread 2 received: " + received);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        t1.start();
        t2.start();
    }


    // 4. Phaser
    // Synchronize threads across multiple phases.
    static void phaserExample() {
        System.out.println("\n--- Phaser ---");

        Phaser phaser = new Phaser(3);

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            new Thread(() -> {
                System.out.println("Worker " + id + " - Phase 1");

                phaser.arriveAndAwaitAdvance();

                System.out.println("Worker " + id + " - Phase 2");

                phaser.arriveAndAwaitAdvance();

                System.out.println("Worker " + id + " finished");

                phaser.arriveAndDeregister();
            }).start();
        }
    }


    // 5. Semaphore
    // Limit the number of threads accessing a resource.
    static void semaphoreExample() {
        System.out.println("\n--- Semaphore ---");

        // Only two threads can access the resource simultaneously.
        Semaphore semaphore = new Semaphore(2);

        for (int i = 1; i <= 5; i++) {
            final int id = i;

            new Thread(() -> {
                boolean acquired = false;

                try {
                    semaphore.acquire();
                    acquired = true;

                    System.out.println("Worker " + id + " accessing resource");

                    Thread.sleep(1000);

                    System.out.println("Worker " + id + " releasing resource");

                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    if (acquired) {
                        semaphore.release();
                    }
                }
            }).start();
        }
    }


    // 6. ThreadLocalRandom
    // Each thread gets its own random number generator.
    static void threadLocalRandomExample() {
        System.out.println("\n--- ThreadLocalRandom ---");

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            new Thread(() -> {
                int randomNumber =
                        ThreadLocalRandom.current().nextInt(1, 101);

                System.out.println(
                        "Thread " + id + " generated: " + randomNumber
                );
            }).start();
        }
    }


    // 7. ReentrantReadWriteLock
    // Multiple readers OR one exclusive writer.
    static void reentrantReadWriteLockExample() {
        System.out.println("\n--- ReentrantReadWriteLock ---");

        ReentrantReadWriteLock lock =
                new ReentrantReadWriteLock();

        int[] sharedData = {0};

        // Reader thread
        Thread reader = new Thread(() -> {
            lock.readLock().lock();

            try {
                System.out.println("Reading: " + sharedData[0]);
            } finally {
                lock.readLock().unlock();
            }
        });

        // Writer thread
        Thread writer = new Thread(() -> {
            lock.writeLock().lock();

            try {
                sharedData[0]++;
                System.out.println("Writing: " + sharedData[0]);
            } finally {
                lock.writeLock().unlock();
            }
        });

        reader.start();
        writer.start();
    }


    // Main method
    public static void main(String[] args) throws InterruptedException {

        countDownLatchExample();

        cyclicBarrierExample();

        exchangerExample();

        phaserExample();

        semaphoreExample();

        threadLocalRandomExample();

        reentrantReadWriteLockExample();
    }
}
'''

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

# Java Lock Interface

The **`Lock` interface** (`java.util.concurrent.locks`) provides explicit locking mechanisms for controlling access to shared resources in multithreaded applications. Unlike the `synchronized` keyword, it offers additional functionality such as non-blocking lock acquisition (`tryLock()`), interruptible locking (`lockInterruptibly()`), and optional timed lock acquisition. Locks must generally be released explicitly using `unlock()`, preferably inside a `finally` block to prevent deadlocks.

## Important Methods

| Method | Description |
|---|---|
| `lock()` | Acquires the lock. Blocks until it becomes available. |
| `unlock()` | Releases the lock. |
| `tryLock()` | Attempts to acquire the lock without blocking. Returns `true` if successful. |
| `tryLock(long time, TimeUnit unit)` | Attempts to acquire the lock within a specified timeout. |
| `lockInterruptibly()` | Acquires the lock unless the thread is interrupted while waiting. |
| `newCondition()` | Creates a `Condition` associated with the lock for thread coordination. |

## Example: ReentrantLock

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Counter {

    private int count = 0;
    private final Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock();

        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
}
```

## Lock vs. synchronized

| Feature | `synchronized` | `Lock` |
|---|---|---|
| Lock acquisition | Automatic | Explicit (`lock()`) |
| Lock release | Automatic | Explicit (`unlock()`) |
| Non-blocking acquisition | No | Yes (`tryLock()`) |
| Timed acquisition | No | Yes |
| Interruptible acquisition | No | Yes |
| Multiple conditions | Limited to one monitor wait set | Multiple `Condition` objects |
| Reentrant | Yes | Depends on implementation; `ReentrantLock` is reentrant |
| Automatic release | Yes | No |

## Important Implementations

| Implementation | Description |
|---|---|
| `ReentrantLock` | Exclusive, reentrant lock with optional fairness policy. |
| `ReentrantReadWriteLock` | Allows multiple concurrent readers or one exclusive writer. |
| `StampedLock` | Supports write locks, read locks, and optimistic reads. It is not reentrant. |

## Important Exam Points

- Always release explicitly acquired locks in a `finally` block.
- `ReentrantLock` allows the same thread to acquire the lock multiple times.
- `tryLock()` does not block while attempting to acquire a lock.
- A lock should only be unlocked by the thread that owns it.
- `Condition` provides `await()`, `signal()`, and `signalAll()` for thread coordination.
