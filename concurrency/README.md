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

```java

import java.util.concurrent.*;
import java.util.concurrent.locks.*;
import java.util.concurrent.ThreadLocalRandom;

public class SynchronizationAids {

    // 1. CountDownLatch
    // Wait until all worker threads have completed.
    static void countDownLatchExample() throws InterruptedException {

        System.out.println("\n--- CountDownLatch ---");

        CountDownLatch latch = new CountDownLatch(3);
        Thread[] threads = new Thread[3];

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            threads[i - 1] = new Thread(() -> {
                System.out.println("Worker " + id + " started");

                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    return;
                }

                System.out.println("Worker " + id + " finished");
                latch.countDown();
            });

            threads[i - 1].start();
        }

        latch.await();

        for (Thread thread : threads) {
            thread.join();
        }

        System.out.println("All workers completed!");

        /*
         EXPECTED OUTPUT (one possible ordering):

         --- CountDownLatch ---
         Worker 1 started
         Worker 2 started
         Worker 3 started
         Worker 1 finished
         Worker 2 finished
         Worker 3 finished
         All workers completed!
        */
    }


    // 2. CyclicBarrier
    // All threads wait until they reach the same barrier.
    static void cyclicBarrierExample() throws InterruptedException {

        System.out.println("\n--- CyclicBarrier ---");

        CyclicBarrier barrier = new CyclicBarrier(3,
                () -> System.out.println(
                        "All workers reached the barrier!"
                )
        );

        Thread[] threads = new Thread[3];

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            threads[i - 1] = new Thread(() -> {
                try {
                    System.out.println("Worker " + id + " preparing");

                    Thread.sleep(500);

                    barrier.await();

                    System.out.println("Worker " + id + " continuing");

                } catch (Exception e) {
                    e.printStackTrace();
                }
            });

            threads[i - 1].start();
        }

        for (Thread thread : threads) {
            thread.join();
        }

        /*
         EXPECTED OUTPUT (one possible ordering):

         --- CyclicBarrier ---
         Worker 1 preparing
         Worker 2 preparing
         Worker 3 preparing
         All workers reached the barrier!
         Worker 1 continuing
         Worker 2 continuing
         Worker 3 continuing
        */
    }


    // 3. Exchanger<V>
    // Two threads exchange objects.
    static void exchangerExample() throws InterruptedException {

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

        t1.join();
        t2.join();

        /*
         EXPECTED OUTPUT:

         --- Exchanger ---
         Thread 1 received: Message B
         Thread 2 received: Message A
        */
    }


    // 4. Phaser
    // Synchronize threads across multiple phases.
    static void phaserExample() throws InterruptedException {

        System.out.println("\n--- Phaser ---");

        Phaser phaser = new Phaser(3);
        Thread[] threads = new Thread[3];

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            threads[i - 1] = new Thread(() -> {

                System.out.println("Worker " + id + " - Phase 1");

                phaser.arriveAndAwaitAdvance();

                System.out.println("Worker " + id + " - Phase 2");

                phaser.arriveAndAwaitAdvance();

                System.out.println("Worker " + id + " finished");

                phaser.arriveAndDeregister();
            });

            threads[i - 1].start();
        }

        for (Thread thread : threads) {
            thread.join();
        }

        /*
         EXPECTED OUTPUT (one possible ordering):

         --- Phaser ---
         Worker 1 - Phase 1
         Worker 2 - Phase 1
         Worker 3 - Phase 1
         Worker 1 - Phase 2
         Worker 2 - Phase 2
         Worker 3 - Phase 2
         Worker 1 finished
         Worker 2 finished
         Worker 3 finished
        */
    }


    // 5. Semaphore
    // Limit the number of threads accessing a resource.
    static void semaphoreExample() throws InterruptedException {

        System.out.println("\n--- Semaphore ---");

        // Only two threads can access the resource simultaneously.
        Semaphore semaphore = new Semaphore(2);

        Thread[] threads = new Thread[5];

        for (int i = 1; i <= 5; i++) {
            final int id = i;

            threads[i - 1] = new Thread(() -> {
                boolean acquired = false;

                try {
                    semaphore.acquire();
                    acquired = true;

                    System.out.println(
                            "Worker " + id + " accessing resource"
                    );

                    Thread.sleep(500);

                    System.out.println(
                            "Worker " + id + " releasing resource"
                    );

                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    if (acquired) {
                        semaphore.release();
                    }
                }
            });

            threads[i - 1].start();
        }

        for (Thread thread : threads) {
            thread.join();
        }

        /*
         EXPECTED OUTPUT (one possible ordering):

         --- Semaphore ---
         Worker 1 accessing resource
         Worker 2 accessing resource
         Worker 1 releasing resource
         Worker 3 accessing resource
         Worker 2 releasing resource
         Worker 4 accessing resource
         Worker 3 releasing resource
         Worker 5 accessing resource
         Worker 4 releasing resource
         Worker 5 releasing resource

         Maximum concurrent access: 2
        */
    }


    // 6. ThreadLocalRandom
    // Each thread gets its own random number generator.
    static void threadLocalRandomExample() throws InterruptedException {

        System.out.println("\n--- ThreadLocalRandom ---");

        Thread[] threads = new Thread[3];

        for (int i = 1; i <= 3; i++) {
            final int id = i;

            threads[i - 1] = new Thread(() -> {
                int randomNumber =
                        ThreadLocalRandom.current().nextInt(1, 101);

                System.out.println(
                        "Thread " + id + " generated: " + randomNumber
                );
            });

            threads[i - 1].start();
        }

        for (Thread thread : threads) {
            thread.join();
        }

        /*
         EXPECTED OUTPUT (example values):

         --- ThreadLocalRandom ---
         Thread 1 generated: 42
         Thread 2 generated: 87
         Thread 3 generated: 13

         Actual random values will vary.
        */
    }


    // 7. ReentrantReadWriteLock
    // Multiple readers OR one exclusive writer.
    static void reentrantReadWriteLockExample()
            throws InterruptedException {

        System.out.println("\n--- ReentrantReadWriteLock ---");

        ReentrantReadWriteLock lock =
                new ReentrantReadWriteLock();

        int[] sharedData = {0};

        // Reader thread
        Thread reader = new Thread(() -> {
            lock.readLock().lock();

            try {
                System.out.println(
                        "Reading: " + sharedData[0]
                );
            } finally {
                lock.readLock().unlock();
            }
        });

        // Writer thread
        Thread writer = new Thread(() -> {
            lock.writeLock().lock();

            try {
                sharedData[0]++;

                System.out.println(
                        "Writing: " + sharedData[0]
                );
            } finally {
                lock.writeLock().unlock();
            }
        });

        reader.start();
        writer.start();

        reader.join();
        writer.join();

        /*
         EXPECTED OUTPUT (one possible ordering):

         --- ReentrantReadWriteLock ---
         Reading: 0
         Writing: 1

         OR:

         --- ReentrantReadWriteLock ---
         Writing: 1
         Reading: 1
        */
    }


    // Main method: execute all examples sequentially.
    public static void main(String[] args)
            throws InterruptedException {

        countDownLatchExample();

        cyclicBarrierExample();

        exchangerExample();

        phaserExample();

        semaphoreExample();

        threadLocalRandomExample();

        reentrantReadWriteLockExample();
    }
}
```

```java

import java.util.concurrent.*;

public class Main {

    public static void main(String[] args) throws Exception {

        // =====================================
        // 1. FIXED THREAD POOL
        // =====================================

        System.out.println("=== Fixed Thread Pool ===");

        ExecutorService fixedPool =
                Executors.newFixedThreadPool(2);

        for (int i = 1; i <= 4; i++) {
            final int task = i;

            fixedPool.submit(() ->
                System.out.println("Task " + task + " executed by "
                        + Thread.currentThread().getName())
            );
        }

        fixedPool.shutdown();
        fixedPool.awaitTermination(5, TimeUnit.SECONDS);

        /*
         * Possible output:
         *
         * === Fixed Thread Pool ===
         * Task 1 executed by pool-1-thread-1
         * Task 2 executed by pool-1-thread-2
         * Task 3 executed by pool-1-thread-1
         * Task 4 executed by pool-1-thread-2
         */


        // =====================================
        // 2. CACHED THREAD POOL
        // =====================================

        System.out.println("\n=== Cached Thread Pool ===");

        ExecutorService cachedPool =
                Executors.newCachedThreadPool();

        for (int i = 1; i <= 3; i++) {
            final int task = i;

            cachedPool.execute(() ->
                System.out.println("Cached task " + task + " executed by "
                        + Thread.currentThread().getName())
            );
        }

        cachedPool.shutdown();
        cachedPool.awaitTermination(5, TimeUnit.SECONDS);

        /*
         * Possible output:
         *
         * === Cached Thread Pool ===
         * Cached task 1 executed by pool-2-thread-1
         * Cached task 2 executed by pool-2-thread-2
         * Cached task 3 executed by pool-2-thread-3
         */


        // =====================================
        // 3. SINGLE THREAD EXECUTOR
        // =====================================

        System.out.println("\n=== Single Thread Executor ===");

        ExecutorService singlePool =
                Executors.newSingleThreadExecutor();

        for (int i = 1; i <= 3; i++) {
            final int task = i;

            singlePool.submit(() ->
                System.out.println("Single task " + task + " executed by "
                        + Thread.currentThread().getName())
            );
        }

        singlePool.shutdown();
        singlePool.awaitTermination(5, TimeUnit.SECONDS);

        /*
         * Possible output:
         *
         * === Single Thread Executor ===
         * Single task 1 executed by pool-3-thread-1
         * Single task 2 executed by pool-3-thread-1
         * Single task 3 executed by pool-3-thread-1
         */


        // =====================================
        // 4. SCHEDULED THREAD POOL
        // =====================================

        System.out.println("\n=== Scheduled Thread Pool ===");

        ScheduledExecutorService scheduledPool =
                Executors.newScheduledThreadPool(2);

        scheduledPool.schedule(
            () -> System.out.println("Delayed task executed"),
            1, TimeUnit.SECONDS
        );

        scheduledPool.shutdown();
        scheduledPool.awaitTermination(5, TimeUnit.SECONDS);

        /*
         * Possible output:
         *
         * === Scheduled Thread Pool ===
         * Delayed task executed
         */


        // =====================================
        // 5. WORK-STEALING POOL
        // =====================================

        System.out.println("\n=== Work-Stealing Pool ===");

        ExecutorService workStealingPool =
                Executors.newWorkStealingPool(2);

        for (int i = 1; i <= 4; i++) {
            final int task = i;

            workStealingPool.submit(() ->
                System.out.println("Work-stealing task " + task
                        + " executed by "
                        + Thread.currentThread().getName())
            );
        }

        workStealingPool.shutdown();
        workStealingPool.awaitTermination(5, TimeUnit.SECONDS);

        /*
         * Possible output:
         *
         * === Work-Stealing Pool ===
         * Work-stealing task 1 executed by ForkJoinPool-1-worker-1
         * Work-stealing task 2 executed by ForkJoinPool-1-worker-2
         * Work-stealing task 3 executed by ForkJoinPool-1-worker-1
         * Work-stealing task 4 executed by ForkJoinPool-1-worker-2
         */


        // =====================================
        // 6. VIRTUAL THREAD PER TASK (JAVA 21+)
        // =====================================

        System.out.println("\n=== Virtual Thread Executor ===");

        try (ExecutorService virtualPool =
                     Executors.newVirtualThreadPerTaskExecutor()) {

            for (int i = 1; i <= 3; i++) {
                final int task = i;

                virtualPool.submit(() ->
                    System.out.println("Virtual task " + task
                            + " executed by "
                            + Thread.currentThread())
                );
            }
        }

        /*
         * Possible output:
         *
         * === Virtual Thread Executor ===
         * Virtual task 1 executed by VirtualThread[#21]
         * Virtual task 2 executed by VirtualThread[#22]
         * Virtual task 3 executed by VirtualThread[#23]
         */


        // =====================================
        // 7. CALLABLE AND FUTURE
        // =====================================

        System.out.println("\n=== Callable and Future ===");

        ExecutorService resultPool =
                Executors.newFixedThreadPool(2);

        Future<Integer> result = resultPool.submit(() -> {
            return 10 + 20;
        });

        System.out.println("Result: " + result.get());

        resultPool.shutdown();

        /*
         * Possible output:
         *
         * === Callable and Future ===
         * Result: 30
         */
    }
}
```


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

# Java Stream API

The **Java Stream API** (introduced in Java 8, `java.util.stream`) provides a functional approach to processing collections of data. It allows filtering, mapping, sorting, reducing and aggregating elements using declarative operations.

Streams do not store data themselves; they process data from a source such as a collection, array or file.

## 1. Main concepts

A stream pipeline consists of three stages:

1. **Source:** Collection, array, file, etc.
2. **Intermediate operations:** `filter()`, `map()`, `sorted()`, `distinct()`.
3. **Terminal operation:** `collect()`, `forEach()`, `reduce()`, `count()`.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

List<Integer> result = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 2)
    .toList();

System.out.println(result);
```

Output:

```text
[4, 8, 12]
```

## 2. Creating streams

| Method | Description |
|---|---|
| `collection.stream()` | Creates a sequential stream |
| `collection.parallelStream()` | Creates a potentially parallel stream |
| `Stream.of()` | Creates a stream from values |
| `Arrays.stream()` | Creates a stream from an array |
| `Stream.empty()` | Creates an empty stream |
| `Stream.generate()` | Generates an infinite stream |
| `Stream.iterate()` | Generates a stream using iteration |
| `Files.lines()` | Creates a stream of file lines |

Examples:

```java
Stream<String> s1 = Stream.of("A", "B", "C");

Stream<Integer> s2 = Arrays.stream(
    new Integer[]{1, 2, 3}
);

Stream<Double> s3 = Stream.generate(Math::random);

Stream<Integer> s4 = Stream.iterate(
    1, n -> n + 1
).limit(5);
```

## 3. Intermediate operations

Intermediate operations transform a stream and return another stream.

They are **lazy**, meaning they are not executed until a terminal operation is invoked.

| Operation | Description | Example |
|---|---|---|
| `filter()` | Selects elements matching a condition | `filter(n -> n > 5)` |
| `map()` | Transforms each element | `map(n -> n * 2)` |
| `flatMap()` | Flattens nested streams | `flatMap(List::stream)` |
| `distinct()` | Removes duplicates | `distinct()` |
| `sorted()` | Sorts elements | `sorted()` |
| `limit()` | Keeps first N elements | `limit(5)` |
| `skip()` | Skips first N elements | `skip(2)` |
| `peek()` | Performs an action while elements pass through | `peek(System.out::println)` |
| `takeWhile()` | Takes elements while condition is true | `takeWhile(n -> n < 10)` |
| `dropWhile()` | Drops elements while condition is true | `dropWhile(n -> n < 10)` |

### filter()

Selects elements based on a predicate.

```java
List<Integer> result = numbers.stream()
    .filter(n -> n > 3)
    .toList();
```

Output: `[4, 5, 6]`

### map()

Transforms each element.

```java
List<Integer> result = numbers.stream()
    .map(n -> n * n)
    .toList();
```

Output: `[1, 4, 9, 16, 25, 36]`

### flatMap()

Transforms each element into a stream and flattens the results into one stream.

```java
List<List<Integer>> nested = List.of(
    List.of(1, 2),
    List.of(3, 4)
);

List<Integer> result = nested.stream()
    .flatMap(List::stream)
    .toList();
```

Output: `[1, 2, 3, 4]`

### distinct(), sorted(), limit() and skip()

```java
List<Integer> result = Stream.of(5, 2, 2, 4, 1, 3)
    .distinct()
    .sorted()
    .skip(1)
    .limit(3)
    .toList();
```

Output: `[2, 3, 4]`

## 4. Terminal operations

Terminal operations trigger stream execution and produce a result or side effect.

| Operation | Description | Return type |
|---|---|---|
| `forEach()` | Performs an action on each element | `void` |
| `toList()` | Collects elements into an unmodifiable list | `List<T>` |
| `collect()` | Collects elements using a collector | Depends on collector |
| `count()` | Counts elements | `long` |
| `reduce()` | Combines elements into one result | `Optional<T>` or other |
| `min()` | Finds minimum element | `Optional<T>` |
| `max()` | Finds maximum element | `Optional<T>` |
| `findFirst()` | Returns first element | `Optional<T>` |
| `findAny()` | Returns any element | `Optional<T>` |
| `anyMatch()` | Checks whether any element matches | `boolean` |
| `allMatch()` | Checks whether all elements match | `boolean` |
| `noneMatch()` | Checks whether no elements match | `boolean` |

### forEach()

```java
numbers.stream()
    .forEach(System.out::println);
```

### count()

```java
long count = numbers.stream()
    .filter(n -> n > 3)
    .count();
```

Output: `3`

### reduce()

Combines all elements into a single result.

```java
int sum = numbers.stream()
    .reduce(0, Integer::sum);
```

Output: `21`

### min() and max()

```java
int min = numbers.stream()
    .min(Integer::compareTo)
    .orElseThrow();

int max = numbers.stream()
    .max(Integer::compareTo)
    .orElseThrow();
```

### anyMatch(), allMatch() and noneMatch()

```java
boolean any = numbers.stream().anyMatch(n -> n > 5);
boolean all = numbers.stream().allMatch(n -> n > 0);
boolean none = numbers.stream().noneMatch(n -> n < 0);
```

Output:

```text
true
true
true
```

## 5. Collectors

`Collectors` is a utility class providing implementations of the `Collector` interface. It is commonly used with `collect()`.

| Collector | Description |
|---|---|
| `toList()` | Collects into a list |
| `toSet()` | Collects into a set |
| `toMap()` | Collects into a map |
| `joining()` | Joins strings |
| `counting()` | Counts elements |
| `summingInt()` | Calculates integer sum |
| `averagingInt()` | Calculates average |
| `groupingBy()` | Groups elements by a property |
| `partitioningBy()` | Divides elements into two groups |
