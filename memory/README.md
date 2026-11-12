# Java Garbage Collectors

Java provides several garbage collectors (GCs), each optimized for different application requirements, such as throughput, low latency, or memory efficiency.

## 1. Overview of Java Garbage Collectors

| Garbage Collector | Main characteristics | Optimization goal |
|---|---|---|
| **Serial GC** | Single-threaded garbage collection | Small applications, low overhead |
| **Parallel GC** | Multi-threaded garbage collection | High throughput |
| **G1 GC** | Divides heap into regions; performs incremental collection | Balanced throughput and latency |
| **ZGC** | Concurrent collection with very short pauses | Very low latency |
| **Shenandoah GC** | Concurrent compaction and collection | Very low latency |
| **Epsilon GC** | Does not reclaim memory | Testing and performance analysis |

## 2. Detailed Properties

### Serial GC

- Uses a single thread for garbage collection.
- Stops all application threads during collection (Stop-The-World).
- Has relatively low overhead.
- Suitable for small heaps and applications with limited CPU resources.

**Best for:** Small applications and environments with limited resources.

### Parallel GC

- Uses multiple threads for garbage collection.
- Stops all application threads during collection.
- Optimized for high application throughput.
- Can produce relatively long pause times.
- Also known as the *Throughput Collector*.

**Best for:** Applications where throughput is more important than response time.

### G1 GC (Garbage-First)

- Divides the heap into multiple equally sized regions.
- Collects regions with the most reclaimable garbage first.
- Uses multiple threads.
- Performs much of its work concurrently with application threads.
- Compacts memory to reduce fragmentation.
- Attempts to meet a configurable pause-time target.
- Suitable for large heaps.

**Best for:** General-purpose applications requiring a balance between throughput and pause times.

### ZGC (Z Garbage Collector)

- Performs most garbage collection work concurrently.
- Designed for extremely short pause times.
- Supports very large heaps.
- Uses colored pointers and load barriers.
- Can compact memory concurrently.
- Trades some throughput and CPU overhead for low latency.

**Best for:** Applications requiring very low latency.

# Escape analysis
Escape Analysis is a JVM optimization technique performed by the Just-In-Time (JIT) compiler to determine whether an object can be accessed outside the method or thread in which it was created. If an object does not escape its scope, the JVM may optimize its allocation using scalar replacement, eliminating the object and replacing its fields with local variables. Escape analysis can also enable lock elision, removing unnecessary synchronization when an object cannot be shared between threads. These optimizations reduce heap allocations, garbage collection overhead, and synchronization costs. Importantly, escape analysis does not necessarily allocate objects on the stack; it can eliminate object allocations entirely.

# Heap memory regions
Heap Memory Regions in Java are the areas of memory where objects and arrays are allocated. In the HotSpot JVM, the heap is divided into several regions to optimize garbage collection. The main regions are **Young Generation**, where newly created objects are allocated, and **Old Generation** (Tenured Generation), where long-lived objects are stored. The Young Generation is further divided into **Eden Space**, where most new objects are allocated, and **two Survivor Spaces** (S0 and S1), where objects that survive garbage collection are moved. Objects that survive multiple garbage collection cycles may eventually be promoted to the Old Generation. Garbage collection in the Young Generation is called Minor GC, while collection involving the Old Generation is commonly associated with Major GC or Full GC, depending on the collector. The exact heap layout and terminology depend on the garbage collector; for example, G1 divides the heap into multiple equal-sized regions rather than using fixed contiguous generations.

Card Table (Garbage Collection) is a data structure used by the JVM garbage collector to track references between different regions of the heap, particularly references from the Old Generation to the Young Generation. The heap is divided into small memory blocks, and each block has an associated card-table entry indicating whether it may contain references to objects in another generation. When an object in the Old Generation is modified to reference an object in the Young Generation, the corresponding card is marked as dirty. During a Young Generation garbage collection, the JVM can scan only the dirty cards instead of scanning the entire Old Generation. This optimization reduces garbage collection overhead and avoids repeatedly scanning large portions of the heap.

# Types of References in Java

Java has four types of object references, which differ in how the garbage collector treats the referenced objects.

| Reference type | Description | Garbage collection |
|---|---|---|
| **Strong Reference** | Normal reference created when assigning an object to a variable. | Object is not collected while strongly reachable. |
| **Soft Reference** | Reference created using `SoftReference<T>`. Useful for memory-sensitive caches. | Object may be collected when the JVM needs memory. |
| **Weak Reference** | Reference created using `WeakReference<T>`. Useful for canonicalized mappings and certain caches. | Object can be collected during the next GC cycle if only weakly reachable. |
| **Phantom Reference** | Reference created using `PhantomReference<T>`. Used for post-mortem cleanup and resource management. | Enqueued after the object becomes phantom reachable and its finalization (if any) is complete. |

## Examples

```java
// Strong reference
Object strong = new Object();

// Soft reference
SoftReference<Object> soft =
    new SoftReference<>(new Object());

// Weak reference
WeakReference<Object> weak =
    new WeakReference<>(new Object());

// Phantom reference
ReferenceQueue<Object> queue = new ReferenceQueue<>();
PhantomReference<Object> phantom =
    new PhantomReference<>(new Object(), queue);
```

## Reference Strength Hierarchy

From strongest to weakest:

1. **Strong** – prevents garbage collection.
2. **Soft** – retained as long as possible, subject to memory needs.
3. **Weak** – does not prevent garbage collection.
4. **Phantom** – allows notification after an object becomes phantom reachable; cannot retrieve the object using `get()`.

## Important Notes

- Soft, weak and phantom references are available through the `java.lang.ref` package.
- Phantom references always return `null` from `get()`.
- A reference queue (`ReferenceQueue`) can be used to detect when soft, weak or phantom references are enqueued.

