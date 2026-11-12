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

```bash
java -XX:+UseSerialGC MyApp
```

### Parallel GC

- Uses multiple threads for garbage collection.
- Stops all application threads during collection.
- Optimized for high application throughput.
- Can produce relatively long pause times.
- Also known as the *Throughput Collector*.

**Best for:** Applications where throughput is more important than response time.

```bash
java -XX:+UseParallelGC MyApp
```

### G1 GC (Garbage-First)

- Divides the heap into multiple equally sized regions.
- Collects regions with the most reclaimable garbage first.
- Uses multiple threads.
- Performs much of its work concurrently with application threads.
- Compacts memory to reduce fragmentation.
- Attempts to meet a configurable pause-time target.
- Suitable for large heaps.

**Best for:** General-purpose applications requiring a balance between throughput and pause times.

```bash
java -XX:+UseG1GC MyApp
```

### ZGC (Z Garbage Collector)

- Performs most garbage collection work concurrently.
- Designed for extremely short pause times.
- Supports very large heaps.
- Uses colored pointers and load barriers.
- Can compact memory concurrently.
- Trades some throughput and CPU overhead for low latency.

**Best for:** Applications requiring very low latency.

```bash
java -XX:+UseZGC MyApp


