# Data structures

## Collection
The Java Collection Framework is a set of classes and interfaces in Java that provides a standard way to store, manage, and manipulate groups of objects. It includes interfaces such as List, Set, Queue, and Map, each designed for different purposes: a List stores ordered elements and allows duplicates, a Set stores unique elements, a Queue manages elements in a specific processing order, and a Map stores data as key-value pairs. The framework also provides implementations such as ArrayList, LinkedList, HashSet, TreeSet, and HashMap, along with utility methods for sorting, searching, and iterating over elements, making data management more efficient and reusable in Java applications.

## Iterable
Java Iterable is an interface in the Java programming language that represents a collection of elements that can be traversed or iterated over one at a time. It provides the iterator() method, which returns an Iterator that allows elements to be accessed sequentially using methods such as hasNext() to check whether more elements are available and next() to retrieve the next element. The Iterable interface also enables the use of the enhanced for-each loop, making it easier to iterate over collections without manually managing an iterator. Interfaces such as List, Set, and Queue extend Collection, which in turn extends Iterable. It is commonly used to provide a consistent way of traversing different types of collections in Java.

## List
A Java List is an interface in the Java Collection Framework that represents an ordered collection of elements, allowing duplicate values and providing access to elements through their index (starting at 0). It supports operations such as adding, removing, updating, searching, and retrieving elements. Java provides several implementations of the List interface, including ArrayList, LinkedList, and Vector, each with different performance characteristics. ArrayList is suitable for fast random access, while LinkedList is useful for frequent insertions and deletions. Lists are commonly used when the order of elements matters and duplicate values need to be maintained.

#### ArrayList
ArrayList is a resizable-array implementation of the Java List interface that stores elements in an internal array and automatically increases its capacity when more elements are added. It maintains insertion order, allows duplicate and null values, and provides fast access to elements using their index. Adding elements at the end is generally efficient, but inserting or removing elements in the middle can be slower because other elements need to be shifted. ArrayList is commonly used when frequent element retrieval is required.

#### AttributeList
AttributeList is a specialized implementation of the Java List interface from the javax.management package. It is primarily used in Java Management Extensions (JMX) to represent a list of attributes associated with a managed object (MBean). It extends ArrayList and provides additional methods for adding and retrieving attribute information. It can store Attribute objects and is commonly used when managing and monitoring Java applications through JMX.

#### CopyOnWriteArrayList
CopyOnWriteArrayList is a thread-safe implementation of the Java List interface from the java.util.concurrent package. Whenever an element is added, removed, or modified, it creates a new copy of its underlying array instead of modifying the existing array. This makes read operations efficient and allows multiple threads to iterate over the list without requiring external synchronization. However, write operations are relatively expensive because they involve copying the array. It is particularly useful in applications where read operations happen frequently and modifications are relatively rare.

#### LinkedList
LinkedList is an implementation of the Java List and Deque interfaces that stores elements as nodes, where each node contains a reference to the previous and next nodes. It maintains insertion order, allows duplicate and null values, and supports operations at both ends of the list. Adding or removing elements is efficient when the required position is already known, but accessing elements by index is slower than with ArrayList because it requires traversing the nodes. LinkedList is commonly used when frequent insertions and removals are needed, or when implementing a queue or deque.

#### RoleList
RoleList is a specialized implementation of the Java List interface from the javax.management.relation package. It extends ArrayList and is designed to store Role objects used in Java Management Extensions (JMX). It provides additional methods for adding and retrieving role information while ensuring that the elements are valid Role objects. It is mainly used in JMX relation management, where roles describe relationships between managed objects (MBeans).

#### RoleUnresolvedList
RoleUnresolvedList is a specialized implementation of the Java List interface from the javax.management.relation package. It extends ArrayList and is designed to store RoleUnresolved objects, which represent roles that could not be properly resolved during JMX relation operations. It provides additional methods for adding and retrieving unresolved role information and is useful for identifying problems when managing relationships between MBeans. It is primarily used in Java Management Extensions for handling incomplete or invalid role information.

#### Stack
Stack is a legacy implementation of the Java List interface that represents a Last-In, First-Out (LIFO) data structure, meaning the last element added is the first one removed. It extends Vector and provides additional methods such as push() to add an element, pop() to remove and return the top element, peek() to inspect the top element, and empty() to check whether the stack contains elements. It is synchronized, making its methods thread-safe individually, but modern Java applications often prefer ArrayDeque for stack operations because it generally offers better performance.

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

## Map
A Java Map is an interface in the Java Collection Framework that stores data as key-value pairs, where each key must be unique but multiple keys can have the same value. It allows efficient insertion, retrieval, updating, and removal of values using their associated keys. Unlike List and Set, Map does not extend the Collection interface. Java provides several implementations of the Map interface, including HashMap, LinkedHashMap, TreeMap, and Hashtable. HashMap provides fast lookups without guaranteeing order, LinkedHashMap maintains insertion order, and TreeMap stores entries sorted by their keys. Maps are commonly used when data needs to be associated with unique identifiers, such as storing user information by ID or maintaining configuration settings.
