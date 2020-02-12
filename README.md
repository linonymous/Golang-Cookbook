# Golang Cookbook

## Contents
* [What is Go](#what-is-go)
    * [Motivation](#motivation)
    * [Features](#golang-features)
    * [Philosophy](#golang-philosophy)
    * [Interpreters and Compilers](#interpreters-and-compilers)
    	* [Compiler](#compiler)
	* [Interpreter](#interpreter)
* [Golang's Type System](#golang-type-system)

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
###### Compiler characteristics:

* Spends a lot of time analyzing and processing the program
* The resulting executable is some form of machine-specific binary code
* The computer hardware interprets (executes) the resulting code
* Program execution is fast

###### Interpreter characteristics:

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

