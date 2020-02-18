# Golang Cookbook 

Motto: A comprehensive document of Golang, with minimalistic, simple explanations of concepts.

## Contents
* [What is Go](#what-is-go)
    * [Motivation](#motivation)
    * [Features](#golang-features)
    * [Philosophy](#golang-philosophy)
    * [Weakness](#weakness)
    * [Interpreters and Compilers](#interpreters-and-compilers)
    	* [Compiler](#compiler)
        * [Interpreter](#interpreter)
        * [Compiler Vs Interpreters](#compiler-vs-interpreter)
            * [Compiler characteristics](#compiler-characteristics)
            * [Interpreter characteristics](#interpreter-characteristics)
* [Golang's Type System](#golang-type-system)
* [Array, Slice and Map](#array-slice-and-map)
    * [Array](#array)
    * [Slice](#slice)
    * [Map](#map)
    * [Memory Leak](#memory-leak)
* [Interface and Structs](#interface-and-struct)
    * [Struct](#struct)
        * [Methods](#methods)
        * [Functions](#functions)
    * [Decoupling using interfaces](#decoupling-using-interface)
    * [Pointer vs Value Receiver](#pointer-vs-value-receiver)
    * [Mutability and Immutability](#mutability-and-immutability)
    * [Embeddings](#embeddings)
    * [Method Promotion Rules](#method-promotion-rules)
* [make vs new](#make-vs-new)
    * [make](#make)
    * [new](#new)
* [Leaky Buffer](#leaky-buffer)
* [Error Handling](#error-handling)
    * [Error](#error)
    * [Panic](#panic)
    * [Recover](#recover)
    * [Defer](#defer)
* [Goroutines](#goroutines)
    * [What are Goroutines?](#what-are-goroutines)
    * [Advantages of Goroutines](#advantages-of-goroutines)
    * [Goroutines Internals](#goroutines-internals)
        * [Thread Basics](#thread-basics)
        * [Multiplexing Threads](#multiplexing-threads)
        * [Efficiency of Goroutine](#efficiency-of-goroutine)
        * [Concurrency vs Parallelization](#concurrency-vs-parallelization)
        * [Data Race](#data-race)
        * [Synchronization](#synchronization)
             * [Sync.Mutex](#syncmutex)
             * [Sync.WaitGroup](#syncwaitgroup)
        * [How many goroutines you can run](#how-many-goroutines-you-can-run)	     
* [Channels](#channels)
     * [What are Channels?](#what-are-channels)
     * [Types of channels and usage](#types-of-channels-and-usage)
        * [Buffered Channels](#buffered-channels)
        * [Unbuffered Channels](#unbuffered-channels)
        * [Usage](#usage)
* [An import with a blank identifier](#an-import-with-a-blank-identifier)
* [Type Assertions](#type-assertions)
    * [Usage](#usage)

TODO: 
- Memory Management Golang
	- Stack & Heap memory management	
	- Garbage Collection
	- Escape Analysis
- dependency management
		- gomodules - submodules
		- godep
- Packaging basics
	- Package designing
	- workflow basics
	- Init function
	- main function 

- - - -

### What is Go?

* Go is a programming language created by Google. It’s open-source and free.
* Go is statically typed compiled language.
* A compiled language is a programming language whose implementations are typically compilers (translators that generate machine code from source code)
* It has support for concurrency from its roots. 
* SYP (Simple Yet Performant)

	[Beauty of GO](https://hackernoon.com/the-beauty-of-go-98057e3f0a7d)
	
	[Introduction to Go](https://medium.com/rungo/introduction-to-go-programming-language-golang-89d16ca72bbf)
	
#### Motivation 
Modern application demands
* Efficiency(optimized memory/process management)
* Scalability(how well it scales across processors, systems)
* Reliability(how crash-proof the code is) and 
* Simplicity (simple to code/understand)
	- Java is highly reliable and scalable, but lacks efficiency and lose simplicity for large applications
	- Python is simple, but not so efficient (does not have intrinsic concurrency support) and reliable (dynamically typed).
	- C, C++ are great at reliability, scale and efficiency but lack simplicity(double pointers, function pointers, etc.).

#### Golang features
* Strongly/statically Typed
* Faster builds/compilations
* Portable
* Intrinsic Concurrency support
* Interfaces (decoupled, simple code)
* Garbage Collection
* No Exceptions (subjective): handle error yourself
* Tooling support: test, profiling, benchmarking, formatting, godoc
* Great built-in libraries: net/http, database, ioutil

#### Golang philosophy
		
* ease of use (simplicity)
* performance (efficiency)
* Strongly/statically typed (reliability)
* First-class concurrency & multithreading support (scalability)
	- was developed as an alternative to c++ & c
	- fast compilation/builds

#### Weakness

* Lack of generics

#### Interpreters and Compilers
* Interpreters and compilers are very similar in structure. The main difference is that an interpreter directly executes the instructions in the source programming language while a compiler translates those instructions into efficient machine code.
##### Compiler
(HLL - Machine Understandable code (assembly) - Machine)
- Lexical Analysis - find lexemes & tokens (keywords, identifier, operators in language)
- Syntax Analysis - generates Abstract syntax tree(AST) using a sequence of tokens and validates the syntax
- Semantic Analysis - Type inference, type checking
- Intermediate Code Generation - From AST, intermediate code is generated, ex 3 address code, where at most three operands can be in a statement
- Optimization - Optimize the code, convert into shorter and performant code
- Final code generation - Translates the optimized intermediate code into assembly or machine-dependent code

##### Interpreter
- The interpreter also translates the program, but it does it one statement at a time

##### Compiler Vs Interpreter
###### Compiler characteristics

* Spends a lot of time analyzing and processing the program
* The resulting executable is some form of machine-specific binary code
* The computer hardware interprets (executes) the resulting code
* Program execution is fast

###### Interpreter characteristics

* Relatively little time is spent analyzing and processing the program
* The resulting code is some sort of intermediate code
* The resulting code is interpreted by another program
* Program execution is relatively slow

- - - - 

### Golang Type System
- A type of variable depicts the fundamental properties the variable is going to have. Some of these properties are, the size of the variable in memory, set of values that are allowed to be assigned to the variable, and operations allowed on the variable.
- Builtin types (int, float, bool, string)
- Composite types (map, slices, array, channel)
- User-defined types (struct, interfaces)
[Golang Types](https://go101.org/article/type-system-overview.html)

- - - -

### Array, Slice and Map
#### Array
- Two byte data structure
- static allocation, size can’t be increased/decreased
- size is the part of type itself, thus [3]int is different than [5]int
- First byte is the pointer to array(address)
- Second byte is the fixed length of array
#### Slice
- Slice is the extension to the arrays
- Dynamic allocation
- Slice wraps around arrays to give flexibility
- Slice holds references to an underlying array
- Three-byte data structure, the first byte is the pointer
- the second byte is the length, indicating, number of elements present in the slice.
- Third byte is capacity, indicating a number of elements in an underlying array, starting from the first element in the slice.
- builtin append function takes slice as the first argument and the element to be appended as the second argument
- When the size of slice exceeds during append, a new static array with size *= 2 is allocated at the backend, all the previous elements are copied and a new slice is returned back with appended data.

	> SLICES ARE DYNAMIC WINDOWS TO STATIC ARRAYS

#### Map
- Is a data structure that maps keys to values
- Keys are unique, and these keys uniquely identify values associated
- KeyType can be any type that is comparable
- Maps in golang are implemented as a hash table
- There is an underlying array, where each element is the bucket, the number of elements in this array are always the power of two.
- Hash is calculated against the key, and lower-order bits of the hash are used to choose the bucket in the array.
- Once the bucket is chosen, each bucket is associated with two data structures at the backend, the first data structure is in array of higher-order bits of hash to distinguish the keys in the same bucket. Second one is the byte array that stores the keys and values in a byte array.
- https://www.ardanlabs.com/blog/2013/08/understanding-slices-in-go-programming.html
- https://blog.golang.org/go-slices-usage-and-internals
- https://www.ardanlabs.com/blog/2013/12/macro-view-of-map-internals-in-go.html	   

#### Memory leak

- Happens when unrequited memory is present (and GC can not clean it)
- Create a slice of a user-defined type
- point a variable to any element at any index
- perform append operation till the time capacity doubles
- Now, a new slice will be allocated and the data will be copied, but the pointer variable in step 2, will still point to the old copy in the old slice, GC would not delete old slice. And causes memory leak problem in golang

https://medium.com/dm03514-tech-blog/sre-debugging-simple-memory-leaks-in-go-e0a9e6d63d4d

### interface and struct

- https://www.ardanlabs.com/blog/2018/03/interface-values-are-valueless.html
- https://www.ardanlabs.com/blog/2017/07/interface-semantics.html
- https://www.ardanlabs.com/blog/2014/05/methods-interfaces-and-embedded-types.html

#### Struct
- struct, like C, is a way to group types together to create another user-defined type.
- It has named fields, The type keyword introduces the new type, and the struct keyword lays out the intrinsic property of type to have different fields inside.
- unnamed struct fields can be accessed with type names. A.int, a.B, etc.
- Anonymous structs with the same fields can be assigned to name struct without explicit type conversion, whereas, two named structs can not be assigned to each other without explicit type conversion. 

- TODO: WRITE STRUCTS memory allocation

##### Methods
- A method in golang is the function defined on the receiver. A receiver might be a pointer receiver or a value receiver.
- Variable of type struct and *struct, are able to access all the methods, with pointer receiver as well as value receiver
- Whereas, when it comes to interface, we deal with method sets.
- Struct variable, with type T tends to have methods with value receiver in its method sets.
- Whereas, struct variables with type *T, tends to have methods with value as well as pointer receiver in its method sets.
- This distinction arises because if an interface value contains a pointer *T, a method call can obtain a value by dereferencing the pointer, but if an interface value contains a value T, there is no safe way for a method call to obtain a pointer.
- The method set of the corresponding pointer type *T is the set of all methods with receiver *T or T
- The method set of any other type T consists of all methods with receiver type T.
 
##### Functions
- Functions are normal functions defined without any receiver, which means functions are not bound to any types in general.

#### Decoupling using interface

- Interface is a contract with pre-defined set of methods
- Any type which has its method set as a superset to the interface method set, are said to agree to the contract.
- Interfaces are used to decouple the code, in a way, that an interface with the defined method set abstracts out the minimal behavior from the concrete implementation, giving the freedom to the user to have a generic dependency on basic required methods rather than the concrete struct itself.
- Decoupling using interface   
	https://stackoverflow.com/questions/49077220/package-decoupling-in-go
https://www.sage42.org/2019/01/30/how-to-fix-tightly-coupled-go-code/

#### Pointer vs Value Receiver
- Use pointer receiver, when you want to mutate variables, or when struct is bigger in size, to optimize the memory
- use-value receiver when you don't want to mutate variables.
- The receivers on a type should be consistent.
	https://golang.org/doc/faq#different_method_sets

#### Mutability and Immutability

- Mutability means, when a change is made to a variable, it changes in place.
- immutability is when, a change is made to a variable, a new variable with changes is allocated 
- Thus, methods with a pointer receiver helps when you need to change something in place. Whereas, value receivers are used when you want immutability.
	
#### Embeddings

- interface embedding & struct embedding
- Embedding, as the word says, to embed one thing in another.
- In golang, it is done by adding the field of one type in another
- Interfaces, might embed another interface, and struct might embed another structs, and interface.
- When a struct is embedded, it brings its own methods along with it.
- When a struct embeds an interface means, it can embed any struct that implements the interface. This means the methods are by default promoted to the upper type that implements it. And as the methods are not defined, you can define/override the methods you want.
- When we embed a type, the methods of that type become methods of the outer type(called Method Promotion), but when they are invoked the receiver of the method is the inner type, not the outer one.
-https://stackoverflow.com/questions/24537443/meaning-of-a-struct-with-embedded-anonymous-interface

##### Method Promotion Rules
- Given a struct type S and a type named T, promoted methods are included in the method set of the struct as follows:
- If S contains an anonymous field T, the method sets of S and *S both include promoted methods with receiver T.
- The method set of *S also includes promoted methods with receiver *T. 
- If S contains an anonymous field *T, the method sets of S and *S both include promoted methods with receiver T or *T.
- If S contains an anonymous field T, the method set of S does not include promoted methods with receiver *T.
			
https://golang.org/doc/effective_go.html#embedding

#### make vs new

#### new
- Is a built-in function that allocates memory, but unlike its namesakes in some other languages it does not initialize the memory, it only zeros it. That is, new(T) allocates zeroed storage for a new item of type T and returns its address, a value of type *T. In Go terminology, it returns a pointer to a newly allocated zero value of type T.

#### make 
- Creates slices, maps, and channels only, and it returns an initialized (not zeroed) value of type T (not *T).
- The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. A slice, for example, is a three-item descriptor containing a pointer to the data (inside an array), the length, and the capacity, and until those items are initialized, the slice is nil. For slices, maps, and channels, make initializes the internal data structure and prepares the value for use. 

### Leaky Buffer
- also called as Buffer pools
- Whenever we are using buffers in our code, some functions just use buffers for storage, we allocate them at the start of the function and leave them for the garbage collector to clean. 
- In some applications, this is frustrating for garbage collectors and has a performance impact. Thus, it is recommended to use buffer pools, and allocate them in advance and use them whenever required. These buffers are called leaky buffers or buffer pools. :) 

### Error handling

#### Error
- error is an interface in golang, which has an Error() method that returns a string.
- Whenever unwanted or unexpected happens in the code, an error is returned.
- Go doesn’t have exceptions, but go recommends that every function has an error returned if something happens.
- Ingo error handling is done, by using this error interface. Where we add another field to be returned from a function which is an error interface. If anything unexpected happens inside the function, the method returns the result along with with nonempty, not nil error value from the function.

#### Panic
- Panic is a built-in function that stops the ordinary flow of control and begins panicking. 
- When the function F calls panic, execution of F stops, any deferred functions in F are executed normally, and then F returns to its caller. 
- To the caller, F then behaves like a call to panic. The process continues up the stack until all functions in the current goroutine have returned, at which point the program crashes. Panics can be initiated by invoking panic directly. They can also be caused by runtime errors, such as out-of-bounds array accesses.
- When to use panics
- An unrecoverable error where your program cannot guarantee the state (thus being unrecoverable)
- Or a programmer error (e.g. passing a wrong type)

- https://blog.golang.org/defer-panic-and-recover

#### Recover
- is a built-in function that regains control of a panicking goroutine. 
- Recover is only useful inside deferred functions. 
- During normal execution, a call to recover will return nil and have no other effect. If the current goroutine is panicking, a call to recover will capture the value given to panic and resume normal execution.
- Executing a call to recover inside a deferred function stops the panicking sequence by restoring normal execution and retrieves the error value passed to the call of panic. If recovery is called outside the deferred function, it will not stop a panicking sequence.

#### Defer
- Defer is used to ensure that a function call is performed later in a program’s execution, usually for purposes of cleanup. defer is often used where e.g. ensure and finally would be used in other languages.

- Try   - Catch   - Finally
- Panic - Recover - Defer


### Goroutines

#### What are goroutines?
- Goroutines are functions/methods that run concurrently with other functions/methods.
- Goroutines are lightweight threads, they exist in userspace and are managed by go runtime, a library to manage memory, scheduling, and garbage collection.
- Everything in golang is a part of a goroutine. A simple main function is executed by a goroutine.
- A goroutine is triggered by using the ‘go’ keyword with a function call. 
- Like every thread implementation, for every goroutine, a stack is created, and every stackframe signifies the new function call, function arguments, whereas some shared variables are stored on the heap, whose decision is made by the compiler itself using escape analysis algorithm.

#### Advantages of Goroutines
- Extremely cheap when compared to threads 
- Goroutines are multiplexed over one or more kernel threads, i.e. a system can have thousands of goroutines running in user space and multiplexed on single os thread. Go abstracts out the intricate details of the implementation.
- Golang has default support for communication between goroutines, where, two goroutines can exchange information, trigger events using channels in golang.

#### Goroutines Internals
##### Thread Basics
- In Linux terminology, there are two types of threads, user-space threads(often called as green threads) and kernel space threads.
https://www.andrew.cmu.edu/user/gkesden/ucsd/classes/sp16/cse120-a/applications/ln/cse120-sp16-lecture4.pdf
http://www.cs.iit.edu/~cs561/cs450/ChilkuriDineshThreads/dinesh's%20files/User%20and%20Kernel%20Level%20Threads.html
- Kernel space threads are created and managed by OS. It runs kernel code and is not associated with any userspace process. Kernel maintains a table for all kernel threads. Thus, the scheduler can give priority to a process with a large number of threads over the process with comparatively fewer threads.
- Kernel level threads, on the contrary, are inefficient and slow.
- Kernel-Level threads make concurrency much cheaper than process because much less state to allocate and initialize. However, for fine-grained concurrency, kernel-level threads still suffer from too much overhead in context switching because thread operations still require system calls. Ideally, we require thread operations to be as fast as a procedure call. Kernel-Level threads have to be general to support the needs of all programmers, languages, runtimes, etc. For such fine-grained concurrency we still need "cheaper" threads.
- To make threads cheap and fast, they need to be implemented at the user level. User-Level threads are managed entirely by the run-time system (user-level library).The kernel knows nothing about user-level threads and manages them as if they were single-threaded processes.User-Level threads are small and fast, each thread is represented by a PC, register, stack, and small thread control block. Creating a new thread, switching between threads, and synchronizing threads are done via procedure call. i.e no kernel involvement. User-Level threads are hundred times faster than Kernel-Level threads.

##### Multiplexing Threads
	
- Thus to get the best of both worlds, it's better to have few OS-level threads (Good at concurrency and scheduling, but inefficient for creation, maintenance, context switch, and synchronization because it involves system calls) and many userspace goroutines (lightweight, easy to create, but not known to OS so poor scheduling) multiplexed over them.
Golang does exactly the same. It multiplexes the goroutines over the OS threads.

- Underhood Concurrency(Parallelism for P>1 :P )
In terminology, this is called the PMG model.
	* P: processor
	* M: OS thread
	* G: Goroutine
- There are N threads running on a processor, and M threads multiplexed over them. It’s M: N mapping. M goroutines need to be scheduled on N OS threads that run on at most GOMAXPROCS. numbers of processors(N <= GOMAXPROCS)
Every processor has a LOCAL RUN QUEUE. Goroutines in LRQ are picked up one by one by the scheduler to schedule them on the owner processor of LRQ.
- There is a GLOBAL RUN QUEUE, GRQ. GRQ is shared across all threads. As LRQ is local, thus, scheduler threads do not need the locks over it. But, GRQ locking is mandatory. Any task which is not assigned to any processor lives in GRQ. 
Whenever the scheduler does not find any thread on LRQ, then it performs THREAD STEALING, that, it randomly accesses the LRQs of other processors (locking comes into picture) and steals half of the workload, means half of the goroutines, if not found, searches for goroutines in GRQ. 
- Algorithm
```
runtime.schedule() {
    // only 1/61 of the time, check the global runnable queue for a G.
    // if not found, check the local queue.
    // if not found,
    //     try to steal from other Ps.
    //     if not, check the global runnable queue.
    //     if not found, poll network.
}
```
https://rakyll.org/scheduler/
https://stackoverflow.com/questions/15983872/difference-between-user-level-and-kernel-supported-threads

- Golang scheduler is cooperative and partially preemptive. The cooperative scheduler is one where the process will run till it relinquishes the control on the CPU. Preemptive is one where processes are preempted after the time slice assigned is over. Go runtime makes the preemption, and thus it is nondeterministic.

#### Efficiency of Goroutine
- Less memory consumption (few kilobytes per goroutine)
- Less setup and teardown cost (after all goroutines are in user space)
- Context switch cost is less, as the scheduling is cooperative and non-preemptive.
- Goroutines are cooperatively scheduled. In cooperative scheduling, there is no concept of the scheduler time slice. In such scheduling, Goroutines yield the control periodically when they are idle or logically blocked in order to run multiple Goroutines concurrently. The switch between Goroutines happens only at well-defined points — when an explicit call is made to the Go Runtime scheduler. And those well-defined points are:
- Channels send and receive operations if those operations would block.
- The Go statement, although there is no guarantee that the new Goroutine will be scheduled immediately.
- Blocking syscalls like file and network operations.
- After being stopped for a garbage collection cycle.

#### Concurrency vs Parallelization
- Concurrency is when tasks can start, run and complete in overlapping periods of time, for example, multitasking on a processor.
- Parallelism is when tasks literally run at the same time.
	
#### Data Race

> With great power comes great responsibility!

- Golang is blessed with top-notch concurrent support, but it raises the vulnerability for shared data(i.e. Data becomes inconsistent).
- Shared channels are synchronized for themselves, thus it is not a thing to worry about.
- But, if two goroutines, end up executing same piece of code, with shared variables, the output becomes non-deterministic, depending upon which goroutine ran first and which ran second. 
- These situations, when data is accessed by two or more goroutines, and as the data is shared, the result becomes non-deterministic, are called race conditions.
- Go by default has a tool to detect the race conditions. This is easy, just use “-race” option in go command-line tools.
- Ex. go run -race main.go

#### Synchronization
- The piece of code, vulnerable to data race is known as the critical section. Thus, whenever, two concurrent threads accessing this piece of code, it is necessary to synchronize them. 
- For the same purpose, go has a sync package, that helps in synchronizing goroutines. The most common synchronization primitives are mutex and waitgroups. 

##### Sync.Mutex
- When we want to share a variable or a piece of code amongst goroutines, it is mandatory to lock this critical section with some lock, so that data race condition might not happen and the behavior of the program remains deterministic.
- Sync.Mutex is based upon mutual exclusion
- mutex.Lock() & mutex.Unlock()
##### Sync.WaitGroup
- Sync.WaitGroup provides a goroutine synchronization mechanism in Golang and is used for waiting for a collection of goroutines to finish.
- When we want all goroutine to end to continue ahead in the program, we use sync.WaitGroup, which halts the program using wg.Wait() so that all goroutines registered with wg.Add(1), marks them as completed using
wg.Done()
- This helps in synchronizing multiple goroutines.  


#### How many goroutines you can run

- Considering 2KB size of a single goroutine, and 8GB of memory:

	*	500 goroutines per 1MB </br>
	*    500,000 goroutines per 1GB </br>
	*  4,000,000 goroutines per 8GB </br>


- From the above calculations, it looks like, for goroutines of 2KB size on 8GB system, you may run as much as a million of goroutines.

- Way to calculate memory used, get the memory usage before & after spawning a million of goroutines using runtime.ReadMemStats(). Subtract the old memory usage with latest and divide by the number of goroutines spawned. 
There you get the average memory used per goroutine. :)

### Channels
#### What are Channels?
- Channels are type-safe queues that have intelligence, to control the behavior of any goroutine attempting for sending & receiving on goroutine.
- A channel helps one goroutine to signal an event to another goroutine and are synchronized. 
- A channel is a synchronized communication object used by goroutines to communicate with each other.
- A goroutine can send some value over a channel and another goroutine can receive some value from the same channel.
- Channels are inherently synchronized. :) 
#### Types of channels and usage
- There are two types of channels
##### Unbuffered Channels
- Channels without capacity are known as Unbuffered channels, therefore it requires both goroutines to make the exchange of resources
- When one tries to send resources over a channel, and there’s no goroutine to receive it, the sender goroutine is locked and made to wait.
- Similarly, when one goroutine tries to receive the resource, and there’s no one to send it, the receiver goroutine is locked and made to wait.
##### Buffered Channels
- Buffered channels have the capacity. Thus they behave quite differently.
- When a buffered channel is full, and sender goroutine wants to send a resource over the channel, the sender goroutine is locked and made to wait.
- When a buffered channel is empty, and receiver goroutine wants to receive a resource over a channel, the receiver goroutine is locked and made to wait.
##### Usage
- ‘<-’ operator is used along with channels.
- channel <- data : puts data onto the channel
- data := <- channel : receives data out of channel



### An import with a blank identifier

- Sometimes, we import a package for its side effects and not for its actual purpose. 
- When a package is imported using a blank identifier, the compiler allows us to import it even when nothing from that package is explicitly being used in the current package. 
- And during execution, the init function in the package imported with a  blank identifier still gets executed. It is said to be side effect caused by that particular package

### Type Assertions

- As the name suggests, we can assert if the value held is of a certain type.
- And, there are not many times when we want to assert a type. We only assert on an interface to check if it of expected type or not.

#### Usage
- value.(typename) returns value, bool.
    - bool is true if the assertion is valid, else it is false.
    - if the assertion is valid, then value gets assigned the value of the type it is asserted against. 
- switch value.(typename) { } Switch case also supports type assertion

