Further reading:
- Effective Java
## The Java Collectoins Framework
The Collections Framewokr consists of:
- collection interfaces: Collection, List, Set, Map, etc
- General-purpose implementations: ArrayList, LinkedList, HashSet, HashMap, etc
- Special-purpose implementations:designed for performance characteristics, usage restrictions, or behavior.
- Cocurrent implementations; designed for use in high-concurrency ccontexts
- Wrapper implementations: adding synchronization, immutability, etc
- Abstract implementation: building blocks for custom implementations
- Algorithms: static method that perform useful functions, such as sorting, or randomizing a collection.
- Infrastructure: interfaces to support the collections.
- Array utilities: static methods that perform useful functions on arrays, such as sorting, initializing, or converting to collections.
 
### The collection interface
java.util.Collection is the root interface in the collections hierarchy. It represents a group of Objects.. Primitive type must be boxed for inclusion in a collection. More specific collectin interfaces extend this interface.
It includes methods: 
    - Adding objects to the collection: add(E), addAll(Collection); 
    - Testing size and membersip: size(), isEmpty(), containsAll(Collection), contains(E); 
    - Iterator: iterator(); 
    - Removing members: remove(E), removeAll(Collection), clear(), retainAll(Collection)
    - Generating array representations: toArray(), toArray(T[])
but, It does not say anything about: the order of elements, whehter they can be duplicated, whether they can be null; the types of elements they can contain.
This interface typically used to pass collections around and manipulate them in the most generic way.

### Iterator
The Iterator interface has: hasNext(), next(), remove().

### List
The java.util.list interface extends the Collection interface.
It declares methods for managing an `ordered` collection of objects(a sequence):
- Control where each element is inserted in the list.
- Access elements by their integer index
- Search for elements in the list
- Insert duplicate elements and null values, in most list implementations.
Some additional method provided by List include:
- add(int index, E element)
- get(int index)
- remove(int index)
- set(int index, E element)--replace the element at the specified location
- subList(int fromIndex, int toIndex)--returns as a List a `modifiable` view of the specified portion of the list(that is, changes to the ivew actually affect the underlying list)
Two implementations:
- ArrayList: best for static array
- LinkedList: better performance if you're frequently inserting and deleting elements, especially from the middle of the collection. But it's slower than an ArrayList for random-access.
Arrays.sort(array);
Collections.sort(collection); // inplace-sorting.

### The Set and SortedSet interfaces
The java.util.Set extends the collection interfaces.
It makes no guarantee of the order of elements, contains at most one `null`, and mutable elements should not be changed while in a Set. It adds no method beyond those of the Collection interface.
The java.util.SortedSet interface extends Set so that elements are ordered:
- A SortedSet Iterator traverse the Set in order.
- A SortedSet also adds teh methods first(), last(), headSet(), tailSet(), and subSet()

And other Set implementations:
- HashSet: A hash table implementation, the best all-around implementation, providing fast lookup and updates.
- LinkedHashset: A hash table and linked list implementation of the Set interface. It maintains the insertion order of elements for iteration, and run nearly as fast as HashSet.
- TreeSet: a red-black tree implementation of the `SortedSet` interface, maintaining the collection in sorted order, but slower for lookups and updates.

### The Queue Interface
It is a collection designed for holding elements prior to processing
- additional insertion, extraction, and inspection.
- can implement FIFO(queue), LIFO(stack), and priority ordering.
- generally do not accept `null`.
Include:
- Inserting elements: add() and offer()
- remvoing and returning elements: remove() and poll()
- returning els witout removal: element() and peek()
The java.util.concurrent.BlockingQueue interfaces declare additional blocking put() and take() methods, designed for cocurrenct access by multiple threads.
General-purpose Queue implementaions:
- LinkedList
- PriorityQueue
Concurrency-specific BlockingQueue implementations:
- ArrayBlockingQueue
- ConcurrentLinkedQueue
- DelayQueue
- LinkedBlockingQueue
- PriorityBlockingQueue
- SynchronousQueue

### The Map Interface
The java.util.Map interface does not extend Collection interface!
methods include:
- Adding key-value pairs to the collection: put(K,V), putAl(Map)
- Retrieving a value by its key: get(K)
- testing size: size(), isEmpty()
- membership: containsKey(K), containsValue(V)
- Removing members: remove(K), clear()
Sortedmap interface extends Map so that elements are automatically ordered by key.
- Iterating over a Sortedmap interates over the els in key order.
- A Sortedmap also adds the methods firstKey(), lastKey(), tailMap(), and subMap()
retrieving Map views: keySet(), values(), entrySet(). Changes to the map are reflected in views and vice-versa.
Iterating over map is done with a Set of objects implementing the java.util.Map.Entry interface. 
    Map.Entry entry: map.entrySet(); entry.getKey(); entry.getValue();
Implementaions:
- HashMap: a hash table implementation, providing fast lookup and updates
- LinkedHashmap: a hash table and linked list implementation of the Map interface. It maintains the `insertion` order of els for iteration, and runs as fast as a HashMap.
- Treemap: a red-black tree implementation of the SortedMap interface, maintaining the collection in `sorted` order, but slower for lookups and updates.

### The Collections class
Collections consists of static method that operate or return collections:
- Taking a collection and returning an unmodifiable view of the collection.
- Taking a collection and returning a synchronized view of the collection for thread-safe use
- returning the minimum or maximum element from a collection
- Sorting, revseriing, rotating, and randoml shuffling List elements.
Severap methods ot the Collections class require objects in the collection to be comparable to implement the Comparable interface, ahving a compareTo() method.

### Type safety in Java Collections
### Java Generics
With the `type` parameter, the compiler ensures that we use the collection with objects of a compatible type only. And no casting is needed. Object type errors are detected at compile time, rather than throwing casting exceptions at runtime.
### Generics and Polymorphism
The collections of a type are `not` polymorphic on the type;
e.g., List<String> = List<Object>  // ***Wrong***, which is a compiler error.
Evenif Employee is a subclass of Person, a List<Employee> can't be assigned to a List<Person>.
### Type Wildcards
The `?` type parameter is intepreted as "type unknown"
- so declaring a variable List<?> means that you can assign a List of any type of object to the reference variable.
- However, once assigned to the variable, you  can't make any assumptoins about the type of the objections in the list. At best you know everything can be treated as an Object.
List<? extends Person> says that the list can be of all Person objects, or all objets can of (the same) subclass of Person:
- So you can access any existing object in the list as Person.
- However, you can't `add` new objects to the List.
- The list contain all Person or Employee, or Customer. So you cannot add any into it.
List< ? super Employee> means a List of objects of type Employee or some supertype of Employee
- So the type is "unknown", but in this case it could be Employee, Person, or Object.
- Because you don't know which, for "read" access the best you can do is use only `Object` methods.
- But you can add new Employee objects to the list, because polymorphism allow the Employee to be treated as a Person or Object as well.
    List<Dog> kennel = new ArrayList<>();
    List<? extends Animal> objs = kennel;
### Generic Methods
A generic method is one implemented to work with a variety of types:
- The method definition contains one or more type parameters whose values are determined when the method is invoked.
- Type parameters are listed within <> after any access modifiers and before the method reutrn type.
- You can use any identifier for a type parameter, but single letters like <T> are used most often.
    static <T> void add(T p, Collection<T> c)
- You can use wildcards, extends, and super in generic method definitions.
    public static <U,T extends U> void add(T obj, Collection<U> c)

   public static <T>  void add1(T obj, Collection<? super T> c) { c.add(obj);}
   > public static \<U,T extends U> void add2(T obj, Collection\<U> c) {c.add(obj);}


# Input/Output
java.io package
### Representing File paths
The java.io.File class is for manipulating file paths.
- the path may or many not refer to an actual on-disk file or directory.
- Methods on the File class alow you to manipulate the path and perform file system operations.
- The File class is not used to read or write file contents.
- The File constructor is overloaded, alloing you to create a File object from:
    - A single String representing a path
    - A String or File representing a parent directory path and a second String argument representing a child directory or file.
- The path used to create a File object can be absolute or relative to the present working directory. 
- Like Strings, Files are `immutable`.
- Once you create one, you cannot modify the path it represents.
Java 7 introduced a new API for managing file paths based on java.nio.file.Path
- Path is an interface that represents a system dependent file path
- use java.nio.file.FileSystem.getPath(String first, String...more)
But FileSystem is an abstract class, so we typically use java.nio.file.FileSystems.getDefault() to get the same FileSystem that File would operate on.
- While Path can give us its parts, it lacks operations(delete, move, exists) that Files have.
- Use java.nio.file.Files toolbox (static method like Files.delete(Path)) to manipulate Paths on the file-system.
- Directory delimiter is represented by java.io.File.pathSeparatorChar static field.
### Managing File Paths
A File provides methods for querying and manipulating the file system:
- query: canRead(), canWrite(), exists(), isDirectory(), isFile(), isHidden(), getAbsolutePath(), lastModified(), length(), listFiles(), listRoots()
A Path depends on Files for querying and manipulating the file system
- Methods that query the file system include: isReadable(Path), isWritable(Path), isExecutable(Path), exists(Path), isDirectory(Path), isRegularFile(Path), isHidden(Path), getLastModifedTime(Path), size(), newDirectoryStream(), etc
- modify: createFile(Path), createDirecotry(Path), move(Path,Path), delete(Path), deleteIfExists(Path), setAttribute(Path, String, Object), setlastModifiedTime(Path),etc

### Input/Output Class hierarchy
A set of four abstract base classes provide the cor functionality for most of these classes:
- byte-oriented (binary) data: inputStream and outputStream
- Character-oriented data: Reader and Writer
Decorator Patterns:
- Most of I/O classes have contructors that take a Reader(or Writer, or InputStream, or OutputStream), and return another.

### Byte Streams
InputStream:
- read() a single byte, or into a byte array
    returns a int value. Only the low-order 8 bites contains the byte of data. The method returns -1 on reaching the end of the stream(EOF).
- skip() bytes
- mark() and reset() position.
- read(byte[] buf) into a byte array, returning the number of bytes actually read in the range of [0, buflength]
- read(byte[], int off, int len)
- close() the stream
OutputStream(): abstract bytewriter
- write(int) a single byte, or form a byte array. it accepts an int as an argument, but only the low-order 8 bites are written.
- write(byte[] b) the entire buffer of bytes.
- write(byte[] b, int off, int len)
- flush() the stream
- close() the stream

### Chracter Streams
Reader: abstract chracter reader:
- read() a single char, or into a char array
- skip() chars
- mark(), reset()
- close()
Writer:
- write()
- flush()
- close()

### Exception Handling in Java I/O
Almost all I/O class contructors and method can throw a checked exception.
The base class for I/O exceptoin is java.io.IOException and some method throw a subclass of IOException.
close() call should always occurs in a finally block, and it can also throw an IOException.

### Java File I/O classes
FileInputStream, FileOutputStream, FileReader, FileWriter.
Overloaded constructors for each class allow you to specify a file path as either a File object or a string, and can throw FileNOtFoundException except FileWriter if it cann't open the file.
FileWriter and FileReader automatically use the system encoding for character translation. Open file with FileinputStream or FileoutputStream, and then use an inputStreamReader or OutputStreamWriter as an adapter to specify appropriate character set.

### PrintWriter/PrintStream
simplify common text output tasks.
- print() is overloaded to print a String representation of all Java primitive types, and to automaticaly print the toString() of all Objects.
- println() add a platform-specific line terminator.
- format() or printf() printed a formatted represetnation of one, or more Objects.
- never throw an IOExcpetion, instead set an internal flag that can be tested via checkError() method.
System.out and System.err are instances of PrintStream.
The Printwriter and PrintStream constructors are overloaded, which can accept a File object or a String representation a file path to specify an output file.

### Reading from the Terminal
System.in is an instance of an InputStream. Using read() reads a bytes a time, not necessarily a complete character.kj
java.io.BufferedReader buffers characters for more effieicenty, including a readLine().

### Filtered Streams
Pre-defined I/O "fileter" classes:
- Buffering(BufferedReader/Writer/InputStream/OutputStream)
- Checksum handling(java.utiil.zip.CheckedInputStream/OutputStream)
- Compressiof(java.util.zip.GZIPInpuStream/OutputStream..)

### Object Serialization
Java supports teh automatic conversion of objects to and from byte streams.
To be seriliazable:
- implement a marker interface java.io.Serializable
- Contain instance field that are serializable- primitives or other Serializable tpes- except for any fields marked as transient
- Have a no-argument constructor
- (optional but recommened) Implement a static final long filed named serialVersionUID as a "version number" to idenfify changes to the class implementation that are incompatible with serizlized objects of previous versions of the class.
Serialize and deserialize objects with the following classes:
- ObjectOutputStream- WriteObject()
- ObjectInputStream- readObject()

## java.net
### java.net.InetAddress
- Represent an IP address
    - supports IPV4 and v6
    - supports unicast and multicast
- Hostname resolution(DNS lookup)
    - supports caching(positive and negative)
### java.net.NetowrkInterface
- Represent system network interface
- Lookup local listening InetAddresses
### java.net.URL
- Uniform Resource Locator
- Elements: scheme/protocol, host, port, authority, userInfo, path, query, fragment/ref
- Factory for URLConnections
### java.net.URLConnection
- Communication link between the app and the URL
- Built-in support for HTTP and JAR connections
- Read/Write from/to connectoin using I/O streams

### java.net.Socket
End-point for communication between two hosts

### java.net.ServerSocket
- Accpets incoming socket connection
- Once accepted use java.net.Socket for communication
- Supports OS queuing(backlog)
- Support blocking and non-blocking modes

### java.nio.channels.SocketChannel and java.nio.channels.ServerSocketChannel
- NIO-version of Socket and ServerSocket
- Use channels, instead of streams
- Can be configured for to be non-blocking when used with a Selector
- NOt trivial to program, as it requires us to keep state

### java.nio.channels.AsynchronousSocketChannel and AsynchronousServerSocket
- Also NIO, but provides a different(call back) programming model.
- New as of Java 7

## Threading and Concurrency
### Runnable and Thread
To run an object in a thread:
- Implement Runnable's run() and start() in a new Thread
- Extend Thread, override run(), instantiate, and start()
Threads can: join, interrrupt, sleep, yield, be prioritized, stack-dumped, be enumerated, grouped, etc.
### Thread Synchronization
- Use `synchronized` keyword to protect critical sections from concurrent access
    - Do not allow data/structures to be modified by multiple threads simultaneously
    - Can be used o methods or blocks of code
- Synchronization occurs on an object that acts as a monitor for the running threads.
    - Usually, this is the object that is to be protected.
    - Can be any other object.
### More Thread synchronization
- Use Object.wait(), Object.notify(), and Object.notifyAll() to schedule threads(wait on each other). This solve the classic Consumer-Producer problem.
- Methods wait(), notify(), and notifyAll() must be called in synchronized blocks. this.wait(), this.notify()
- Always do checks around wait() using while or for loops rather than simple if conditions.
- better yet, use Java5's java.util.concurrent advanced thread-safe data structures and controls.
### Java 5 Concurrency Features
java.util.concurrency package provides a powerful, extensible framework of high-performance, scalabe, thread-safe building bokcs for developing concurrenct clases and applications.
- Thread pools and custom execution frameworks
- Queues and related thread-safe collections
- Atomic variables
- Special-purpose locks, barriers, semaphores, and conditional variables
Better than synchronized, wait(), notify(),...

Built-in concurrency primitives: wait(), notify(), synchronized are too hard:
    - hard to use
    - easy to make erros
    - low level for most applications
    - poor performance if used incorrectly
    - Too much wheel-reinvening
Concurrency APIs in Java 5:
- java.util.concurrency: Utility classes and frameworks commonly used in concurrent programming-e.g., thread pools
- java.util.concurrenct.atomic - classes that support lock-free and thread-safe operations on single variables- e.g. atomic i++
- java.util.concurrent.locks: Framework for locking and waiting for conditions that is distinct from built-in synchronization and monitors.

### Thread scheduling
Pre-java5: new thread every time - poor resource management 
Thread scheduling in Java 5: Executors.new CachedThreadPool(); pool.execute(new Runnable(){})
Java5's Scheduling frameworks standardize invocations, scheduing, execution, and contorl of asynchronous tasks.
We can use its thread-pool executor service to provide better resource utilization
Executor interface decouples task submission from the mechanics of how it will be run.

`ExecutorService` builds upon `Executor` to provide lifecycle management and support for tracking progress of asynchronous tasks.
Constructed through factory methods on Executors class.
`ScheduleExecutorService` provides a framework for executing deferred and recurring tasks that can be cancelled, and like Timer but supports pooling

#### Gettting Results from Thread pre Java 5
Getting results from operations that run in separate threads requries a lot of boiler-plate code.
We need a custom Runnable that rememters its result(or exception) as its state and we explicitly access it.
What if we neeeded to wait on the result? We need to join the thread.

#### Getting results from Threads in java 5
a new interface runnable returns a result and may throw an exception.
    public interface Callable<V> { public V call() throws Exception;}
Futures make it easy to define asynchronous operations, and ExecutorService.submit(Callable<T> task) conveniently returns Future<T> for that task. Call Future.get() to wait on the result.

#### Thread Sync Pre-Java 5
Java's `synchronized` construct forces all lock acquisition and release to occur in a block-structured way:
- Cannot easily implement chain-locking
- Often used inefficiently
Using Object.wait() and Object.notfiy() for thread sync is extremely hard to do well.

#### Thread Sync in Java 5 with Locks
Locks and conditional variales provides an alternative to implicit object monitors
`ReadWirteLock` allows concurrent "read" access to a shared resource.
Possible to provide deadlock detection and non-reentrant usage semantics.
Support for guarantee fair-ordering: access to longest-waiting thread.

#### Benefits of Java 5 Locks
We can do chain-locking
Non-blocking lock attempts: aLock.tryLock();
Interrupted lock attempts: aLock.lockInterruptibly():
Timing out lock attempts: aLock.tryLock(5, TimeUnit.SECONDS)

#### Java 5 conditionals
Conditions provide means for one thread to suspend its execution until notified by another.
Condition are to the Object class's monitor methods(wait, nofify and notifyAll)
Support for uninterruptiable conditions: Condition.awaitUninterruptibly()
time-wait conditions: Condition.await(long time, TimeUnit unit) returns false if time elapsed.
absolute time waits: Condition.await(Date deadline)

#### Atomics in Java 5
Prior to java 5, operations such as i++ had to be wrapped in synchronized if thread safety was important.
Now we have atomic "primitives":
- Easier and more efficient
- Can be sued in arrays/collections
- Bulding blocks for non-blocking data structures.
AtomicInteger, AtomicLong, AtomicBoolean

#### Other Concurrency Features
Concurrent collections offers thread-safe iteration and better performance than HashTable and Vector.
- ConcurrentHashMap
- CopyOnWriterArrayList
General purpose synchronizers: Latch, Semaphore, Barrier, Exchanger
Nanosecond-granularity timing: aLock.tryLock(250, TimeUnit.NANOSECONDS)
Semaphore- Couting semaphore maintains a set of permits and is generally used to restrict number of threads that can access a resource.
CyclicBarrier- Aids a set of threads to wait for each other to reach a common "barrier" point.
CountDownlatch, allow a set of threads to wait for some number of operations performed by other threads to complete.
Exchanger<V>, a synchronization point at which a pari of threads can exchange objects.

### Promises
Promises are new functional constructs introduced in Java 8.
They work hand-in-hand with lambdas and the standard functional interface allowing interdependent functions to be weaved together and run synchronously or asynchronously.
Promises build upon Futures.
- Both originated from functional programming langages
- both are placeholder for values that are to be computed in the future.
- Both decouple the data provider from the data consumer.
- But Futures are read-only constructs that simply hold values.
- While promises allow complex behavior to be defined once the value is set.
- Promises are futures that are to be compleleted.
in Java 8, Promises are created with the CompletableFuture class.
Applicability:
- Parallel processing of large-scale, multi-stage, CPU-intensive computation.
- Aggregate of results from multiple network calls.
- Anytime you want to avoid "callback hell".
It is also possible to manually thread the execution of the function (or use the main thread).
- However, promises have to be manually fulfilled or rejected.
Fulfill: promise.complete(valueToFulfill)
Reject: promise.completeExceptionally(new MyException())
Check the status: promise.isCompleteExceptionally()
