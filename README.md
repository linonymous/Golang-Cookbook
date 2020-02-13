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
* [Interface and Structs](#interfacce_and_struct)
    * [Struct](#struct)
        * [Methods](#methods)
        * [Functions](#functions)
    * [Decoupling using interfaces](#decoupling_using_interfaces)
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
	
#### Embeddings, struct embeddings, interface embeddings

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
	
