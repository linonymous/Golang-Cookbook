# Golang Cookbook

## Contents
* [What is Go](#what-is-go)
    * [Motivation](#motivation)
    * [Features](#golang-features)
    * [Philosophy](#golang-philosophy)
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
* scalability(how well it scales across processors, systems)
* Reliability(how crash-proof the code is) and 
* simplicity (simple to code/understand)
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
	- Go's Weaknesses
		- Lack of generics

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

	SLICES ARE DYNAMIC WINDOWS TO STATIC ARRAYS

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
