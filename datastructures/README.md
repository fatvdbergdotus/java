# Data structures

## Collection
The Java Collection Framework is a set of classes and interfaces in Java that provides a standard way to **store, manage, and manipulate groups of objects**. It includes interfaces such as List, Set, Queue, and Map, each designed for different purposes: a List stores ordered elements and allows duplicates, a Set stores unique elements, a Queue manages elements in a specific processing order, and a Map stores data as key-value pairs. The framework also provides implementations such as ArrayList, LinkedList, HashSet, TreeSet, and HashMap, along with utility methods for sorting, searching, and iterating over elements, making data management more efficient and reusable in Java applications.

## Iterable
Java Iterable is an interface in the Java programming language that represents a collection of elements that can be **traversed or iterated over one at a time**. It provides the iterator() method, which returns an Iterator that allows elements to be accessed sequentially using methods such as hasNext() to check whether more elements are available and next() to retrieve the next element. The Iterable interface also enables the use of the **enhanced for-each loop**, making it easier to iterate over collections without manually managing an iterator. Interfaces such as List, Set, and Queue extend Collection, which in turn extends Iterable. It is commonly used to provide a consistent way of traversing different types of collections in Java.

## List
A Java List is an interface in the Java Collection Framework that represents an **ordered collection of elements**, allowing duplicate values and providing access to elements through their index (starting at 0). It supports operations such as adding, removing, updating, searching, and retrieving elements. Java provides several implementations of the List interface, including ArrayList, LinkedList, and Vector, each with different performance characteristics. ArrayList is suitable for fast random access, while LinkedList is useful for frequent insertions and deletions. Lists are commonly used when the **order of elements matters** and duplicate values need to be maintained.

#### ArrayList
ArrayList is a resizable-array implementation of the Java List interface that stores elements in an **internal array** and** automatically increases** its capacity when more elements are added. It maintains insertion order, allows duplicate and null values, and provides fast access to elements using their index. Adding elements at the end is generally efficient, but inserting or removing elements in the middle can be slower because other elements need to be shifted. ArrayList is commonly used when **frequent element retrieval** is required.

#### AttributeList
AttributeList is a specialized implementation of the Java List interface from the javax.management package. It is primarily used in Java Management Extensions (JMX) to represent a list of attributes associated with a managed object (MBean). **It extends ArrayList** and provides **additional methods for adding and retrieving attribute information**. It can store Attribute objects and is commonly used when managing and monitoring Java applications through JMX.

#### CopyOnWriteArrayList
CopyOnWriteArrayList is a **thread-safe implementation** of the Java List interface from the java.util.concurrent package. Whenever an element is added, removed, or modified, it **creates a new copy of its underlying array** instead of modifying the existing array. This makes read operations efficient and allows multiple threads to iterate over the list without requiring external synchronization. However, **write operations are relatively expensive** because they involve copying the array. It is particularly useful in applications where read operations happen frequently and modifications are relatively rare.

#### LinkedList
LinkedList is an implementation of the Java List and Deque interfaces that stores elements as nodes, where **each node contains a reference to the previous and next nodes**. It maintains insertion order, allows duplicate and null values, and supports operations at both ends of the list. Adding or removing elements is efficient when the required position is already known, but accessing elements by index is slower than with ArrayList because it requires traversing the nodes. LinkedList is commonly used when **frequent insertions and removals** are needed, or when implementing a queue or deque.

#### RoleList
RoleList is a specialized implementation of the Java List interface from the javax.management.relation package. **It extends ArrayList** and is designed to store **Role objects** used in Java Management Extensions (JMX). It provides **additional methods** for adding and retrieving role information while ensuring that the elements are valid Role objects. It is mainly used in JMX relation management, where roles describe relationships between managed objects (MBeans).

#### RoleUnresolvedList
RoleUnresolvedList is a specialized implementation of the Java List interface from the javax.management.relation package. **It extends ArrayList** and is designed to store RoleUnresolved objects, which represent **roles that could not be properly resolved** during JMX relation operations. It provides additional methods for adding and retrieving unresolved role information and is useful for identifying problems when managing relationships between MBeans. It is primarily used in Java Management Extensions for handling incomplete or invalid role information.

#### Stack
Stack is a legacy implementation of the Java List interface that represents a Last-In, First-Out (LIFO) data structure, meaning the last element added is the first one removed. **It extends Vector** and provides additional methods such as **push()** to add an element, **pop()** to remove and return the top element, **peek()** to inspect the top element, and **empty()** to check whether the stack contains elements. It is **synchronized**, making its methods thread-safe individually, but modern Java applications often prefer ArrayDeque for stack operations because it generally offers better performance.

#### Vector
Vector is a legacy resizable-array implementation of the Java List interface that stores elements in an internal array and automatically increases its capacity when necessary. It maintains insertion order, allows duplicate and null values, and provides indexed access to elements. Unlike ArrayList, its methods are synchronized, making individual operations thread-safe. However, synchronization can introduce additional overhead, so ArrayList is generally preferred when thread safety is not required. Vector is mainly encountered in older Java applications and legacy code.

## Set
A Java Set is an interface in the Java Collection Framework that represents a collection of unique elements, meaning it does not allow duplicate values. Unlike a List, a Set does not provide access to elements through indexes. Java provides several implementations of the Set interface, including HashSet, LinkedHashSet, and TreeSet. HashSet offers fast operations without guaranteeing element order, LinkedHashSet maintains insertion order, and TreeSet stores elements in sorted order. Sets are commonly used when uniqueness is important, such as removing duplicates or efficiently checking whether an element already exists in a collection.

#### ConcurrentHashMap.KeySetView
ConcurrentHashMap.KeySetView is a thread-safe Set implementation provided by ConcurrentHashMap that represents the keys stored in a concurrent hash map. It allows elements to be accessed and iterated over safely by multiple threads without requiring external synchronization. Depending on how it is created, elements can be added to the set, automatically creating corresponding key-value entries in the underlying map using a predefined mapped value. It is useful when multiple threads need to work with a shared collection of unique keys.

#### ConcurrentSkipListSet
ConcurrentSkipListSet is a thread-safe implementation of the Java NavigableSet interface that stores unique elements in sorted order. It uses a concurrent skip-list data structure to provide efficient searching, insertion, and removal while allowing multiple threads to access and modify the set simultaneously. It also supports navigation operations such as retrieving the first, last, lower, higher, and nearest elements. It is commonly used when a sorted collection must be accessed concurrently.

#### CopyOnWriteArraySet
CopyOnWriteArraySet is a thread-safe implementation of the Java Set interface that internally uses a CopyOnWriteArrayList to store unique elements. Whenever an element is added or removed, a new copy of the underlying array is created, allowing read operations and iteration to happen safely without external synchronization. However, modifications are relatively expensive because they require copying the array. It is best suited for applications where reading and iterating happen frequently while modifications are rare.

#### EnumSet
EnumSet is a specialized implementation of the Java Set interface designed exclusively for storing enum constants. It uses a highly efficient bit-vector representation, making operations such as adding, removing, and checking elements very fast while consuming little memory. It maintains the natural declaration order of enum constants and does not allow null values. It is commonly used when working with a predefined group of enum values, such as days of the week, application states, or permissions.

#### HashSet
HashSet is a commonly used implementation of the Java Set interface that stores unique elements using a hash table. It does not guarantee the order of elements and allows one null value. It provides average constant-time performance for basic operations such as adding, removing, and checking whether an element exists, assuming a good distribution of hash codes. It is commonly used when duplicate elements must be avoided and fast lookup is important.

#### JobStateReasons
JobStateReasons is a specialized implementation of the Java Set interface from the javax.print.attribute.standard package. It stores unique JobStateReason values that describe why a print job is in a particular state, such as being held, blocked, or stopped. It extends HashSet and maintains a collection of reasons associated with a printer job. It is primarily used in Java printing applications to provide additional information about the status of print jobs.

#### LinkedHashSet
LinkedHashSet is an implementation of the Java Set interface that combines the uniqueness of a HashSet with predictable iteration order. It uses a hash table and a linked list to maintain elements in insertion order while preventing duplicates. It allows one null value and provides average constant-time performance for basic operations such as adding, removing, and searching. It is useful when a collection needs to contain unique elements while preserving the order in which they were inserted.

#### TreeSet
TreeSet is an implementation of the Java NavigableSet interface that stores unique elements in sorted order using a tree-based data structure, typically a Red-Black Tree. It automatically sorts elements according to their natural ordering or a custom Comparator. It provides efficient operations for adding, removing, and searching elements, generally with logarithmic time complexity. It also supports navigation operations such as retrieving the smallest, largest, lower, and higher elements. It is commonly used when unique elements must always remain sorted.

## Queue
A Java Queue is an interface in the Java Collection Framework that represents a collection of elements designed for processing in a specific order, typically following the First-In, First-Out (FIFO) principle, meaning the first element added is usually the first one removed. It provides operations such as offer() to add elements, poll() to retrieve and remove the head, and peek() to inspect the head without removing it. Java provides several implementations of the Queue interface, including LinkedList, PriorityQueue, and ArrayDeque. LinkedList and ArrayDeque generally follow FIFO ordering, while PriorityQueue processes elements according to their priority rather than insertion order. Queues are commonly used in task scheduling, message processing, and managing requests in applications.

#### ArrayBlockingQueue
ArrayBlockingQueue is a thread-safe, bounded blocking queue that stores elements in a fixed-size array and follows the First-In, First-Out (FIFO) principle. Its capacity must be specified when the queue is created and cannot be changed afterward. When the queue is full, adding elements blocks until space becomes available, and when it is empty, retrieving elements blocks until an element is added. It is commonly used for producer-consumer problems where controlling the number of stored elements is important.

#### ArrayDeque
ArrayDeque is a resizable-array implementation of the Java Deque interface that allows elements to be added and removed from both the beginning and the end of the collection. It can be used as both a queue (FIFO) and a stack (LIFO) and generally provides efficient operations at both ends. Unlike Stack and Vector, it is not synchronized and does not guarantee thread safety. It does not allow null elements and is commonly used when a fast, general-purpose queue or stack is needed.

#### ConcurrentLinkedDeque
ConcurrentLinkedDeque is a thread-safe, unbounded, double-ended queue that allows multiple threads to add and remove elements from both ends without requiring external synchronization. It follows a non-blocking algorithm, meaning threads do not need to wait for locks to access the collection. It supports both FIFO and LIFO operations and does not allow null elements. It is useful in concurrent applications where multiple threads need to efficiently share a deque.

#### ConcurrentLinkedQueue
ConcurrentLinkedQueue is a thread-safe, unbounded queue that follows the First-In, First-Out (FIFO) principle and allows multiple threads to add and remove elements simultaneously. It uses a non-blocking algorithm based on linked nodes, providing efficient concurrent operations without requiring traditional locks. Unlike blocking queues, it does not wait when empty or full, and it does not allow null elements. It is commonly used for asynchronous task processing and communication between multiple threads.

#### DelayQueue
DelayQueue is an unbounded, thread-safe blocking queue in which elements can only be retrieved after their specified delay has expired. Its elements must implement the Delayed interface, which defines how the remaining delay is calculated and how elements are compared. Elements with the shortest remaining delay become available first, rather than necessarily following insertion order. It is commonly used for scheduling tasks, managing timeouts, and implementing expiration-based operations.

#### LinkedBlockingDeque
LinkedBlockingDeque is a thread-safe, optionally bounded, double-ended blocking queue implemented using linked nodes. It allows elements to be inserted and removed from both the beginning and the end of the queue, supporting both FIFO and LIFO operations. When a capacity is specified and the deque is full, insertion operations wait until space becomes available; when it is empty, retrieval operations wait for an element. It is useful in concurrent applications that require flexible producer-consumer communication.

#### LinkedBlockingQueue
LinkedBlockingQueue is a thread-safe, optionally bounded blocking queue implemented using linked nodes and typically follows the First-In, First-Out (FIFO) principle. It allows multiple threads to add and retrieve elements safely. When a specified capacity is reached, insertion operations block until space becomes available, while retrieval operations block when the queue is empty. If no capacity is specified, its capacity defaults to Integer.MAX_VALUE. It is commonly used in producer-consumer applications and thread pools.

#### LinkedTransferQueue
LinkedTransferQueue is a thread-safe, unbounded queue that supports asynchronous and synchronous element transfers between producer and consumer threads. It extends the functionality of a standard blocking queue by providing the transfer() method, which allows a producer to wait until another thread receives the element. It also supports non-blocking operations such as offer() and blocking operations such as take(). It is useful in high-concurrency applications where efficient communication and direct data transfer between threads are required.

#### PriorityBlockingQueue
PriorityBlockingQueue is a thread-safe, unbounded blocking queue that processes elements according to their natural ordering or a custom Comparator, rather than following FIFO insertion order. Elements with the highest priority according to the ordering are retrieved first. When the queue is empty, retrieval operations block until an element becomes available, but insertion operations do not block because the queue has no fixed capacity. It is commonly used for task scheduling and concurrent processing where certain tasks must be handled before others.

#### PriorityQueue
PriorityQueue is a queue implementation that stores elements according to their natural ordering or a custom Comparator, rather than insertion order. It is typically implemented using a binary heap, allowing efficient retrieval and removal of the element with the highest priority according to its ordering. It does not allow null elements, permits duplicate elements, and is not thread-safe. It is commonly used in algorithms such as Dijkstra's shortest path, task scheduling, and situations where elements must be processed according to priority.

#### SynchronousQueue
SynchronousQueue is a special thread-safe blocking queue that does not store any elements because every insertion operation must be directly matched with a corresponding removal operation by another thread. A producer attempting to insert an element waits until a consumer retrieves it, and a consumer attempting to retrieve an element waits until a producer provides one. It can operate in either fair or non-fair ordering mode. It is commonly used for direct handoffs between threads, particularly in thread pools and concurrent producer-consumer applications.

## Map
A Java Map is an interface in the Java Collection Framework that stores data as key-value pairs, where each key must be unique but multiple keys can have the same value. It allows efficient insertion, retrieval, updating, and removal of values using their associated keys. Unlike List and Set, Map does not extend the Collection interface. Java provides several implementations of the Map interface, including HashMap, LinkedHashMap, TreeMap, and Hashtable. HashMap provides fast lookups without guaranteeing order, LinkedHashMap maintains insertion order, and TreeMap stores entries sorted by their keys. Maps are commonly used when data needs to be associated with unique identifiers, such as storing user information by ID or maintaining configuration settings.

#### ConcurrentHashMap
ConcurrentHashMap is a thread-safe implementation of the Java Map interface that allows multiple threads to access and modify key-value pairs concurrently without requiring the entire map to be locked. It provides efficient read and write operations by using fine-grained synchronization and other concurrency mechanisms. It does not allow null keys or values and does not guarantee iteration order. It is commonly used in multithreaded applications where multiple threads need to share and update a map safely.

#### ConcurrentSkipListMap
ConcurrentSkipListMap is a thread-safe implementation of the Java NavigableMap interface that stores key-value pairs in sorted order using a concurrent skip-list data structure. Keys are ordered according to their natural ordering or a custom Comparator, and duplicate keys are not allowed. It supports efficient concurrent insertion, removal, searching, and navigation operations, such as retrieving the lowest, highest, lower, and higher keys. It does not allow null keys or values and is useful when multiple threads need access to a sorted map.

#### EnumMap
EnumMap is a specialized implementation of the Java Map interface designed specifically for using enum constants as keys. It stores entries in the natural order of the enum constants and uses an internal array-based representation, making it highly efficient in terms of performance and memory usage. All keys must belong to the same enum type, while values can be null. It is commonly used when mapping predefined enum values to associated information, such as mapping days of the week to schedules or application states to descriptions.

#### HashMap
HashMap is a commonly used implementation of the Java Map interface that stores data as key-value pairs using a hash table. It provides average constant-time performance for basic operations such as inserting, retrieving, and removing entries, assuming a good distribution of hash codes. It does not guarantee insertion order or sorted order, allows one null key and multiple null values, and is not thread-safe. It is commonly used when fast data retrieval through unique keys is required.

#### Hashtable
Hashtable is a legacy, synchronized implementation of the Java Map interface that stores key-value pairs using a hash table. Its methods are synchronized, allowing individual operations to be performed safely by multiple threads, although this can introduce performance overhead. Unlike HashMap, it does not allow null keys or values and does not guarantee any particular iteration order. It is mainly encountered in older Java applications, while modern applications generally prefer ConcurrentHashMap for concurrent access.

#### IdentityHashMap
IdentityHashMap is a specialized implementation of the Java Map interface that compares keys using reference equality (==) instead of logical equality (equals()). This means two different objects containing identical data are treated as separate keys unless they refer to the exact same object in memory. It uses an internal array-based hash table and allows one null key and multiple null values. It is mainly used in specialized situations such as object graph processing, serialization, and tracking object identity.

#### LinkedHashMap
LinkedHashMap is an implementation of the Java Map interface that combines a hash table with a doubly linked list to maintain a predictable iteration order. By default, it preserves the insertion order of entries, but it can also be configured to maintain access order, which is useful for implementing Least Recently Used (LRU) caches. It provides average constant-time performance for basic operations and allows one null key and multiple null values. It is commonly used when fast key-value lookups and predictable iteration order are both required.

#### TreeMap
TreeMap is an implementation of the Java NavigableMap interface that stores key-value pairs in sorted order using a tree-based data structure, typically a Red-Black Tree. Keys are sorted according to their natural ordering or a custom Comparator, and duplicate keys are not allowed. It provides logarithmic time complexity for basic operations such as insertion, retrieval, and removal. It supports navigation operations such as retrieving the first, last, lower, and higher keys. It is commonly used when key-value entries need to remain sorted automatically.

#### WeakHashMap
WeakHashMap is a specialized implementation of the Java Map interface that stores keys using weak references, allowing keys to be garbage-collected when there are no longer strong references to them elsewhere in the application. When a key is garbage-collected, its corresponding entry can automatically be removed from the map. It provides functionality similar to HashMap, allows one null key and multiple null values, and is not thread-safe. It is commonly used for memory-sensitive caches and situations where objects should not be kept alive solely because they are stored as map keys.
