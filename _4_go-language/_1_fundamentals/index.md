# What is go?

- Go is an open-source, statically typed, compiled programming language developed by Google, designed for simplicity, high performance, and built-in concurrency using goroutines and channels.

# Why Go Was Created?

Google faced problems:

- Slow compilation (C++)
- Complex code (Java)
- Poor concurrency support

So they built Go to solve:

- Fast compilation
- Easy concurrency (goroutines)
- Simple syntax
- High performance

# Why Use Go?

Go is used because:

- Fast Performance -> Go is compiled (like C/C++)
- Built-in Concurrency -> Goroutines make concurrency easy:Very lightweight
- Simple Syntax -> Less code, easier maintenance
- Fast Compilation -> Large projects compile in seconds

# Where Go Is Used (Real Companies)?

Big companies using Go:

- Google
- Uber
- Netflix
- Dropbox
- Docker
- Kubernetes (written in Go)

# Advantages of Go?

- Very Fast -> Compiled language → High performance
- Easy Concurrency -> Goroutines + Channels
- Simple Language
- Fast Compilation -> Large projects compile quickly
- Built-in Standard Library -> like HTTP server, JSON handling, File handling, Concurrency tools
- Memory Efficient -> Uses Garbage Collection

# Disadvantages of Go?

- No Generics (Previously) -> Go added generics in: Go 1.18, But still limited compared to Java/C++
- Error Handling Verbose
  if err != nil {
    return err
  }
  Repeated many times

# Garbage collection

- Go uses garbage collection and hence memory management is pretty much taken care automatically and the developer doesn’t need to worry about managing memory. 
- This also helps to write concurrent programs easily.

# Zero value

- Zero value is the default value assigned to a variable when it’s declared without initialization, like 0 for int, "" for string, false for bool, and nil for pointers or slices.

# var vs :=

- var is used for explicit declaration and can be used globally, while := is a short declaration used inside functions with type inference.

# Is Go object-oriented?

- Go is not fully object-oriented, but it supports OOP concepts such as encapsulation and polymorphism using structs and interfaces.

# Package & main package

- A package is a collection of related Go files.
- The main package is special because it defines the entry point of the application through the main() function.

# init() function

- init() is executed automatically before main() and is mainly used for initialization logic like setting configs or connections.

# Rune

- A rune is an alias for int32 and represents a Unicode character, useful for handling non-ASCII characters.

# Type inference

- Type inference allows the Go compiler to automatically determine the variable type based on the assigned value.

# Constants

- Constants are immutable values defined using const and are evaluated at compile time.

# Unused import

- Go throws a compile-time error if an imported package is not used, enforcing clean and readable code.

# A short explanation of the hello world program

package main
import "fmt"
func main() {
fmt.Println("Hello World")
}

- package main - A package is a collection of related Go files. The main package is special because it defines the entry point of the application through the main() function.

- import “fmt” - The import statement is used to import other packages. In our case, fmt package is imported and it will be used inside the main function to print text to the standard output.

- func main() - The func keyword marks the beginning of a function. The main is a special function. The program execution starts from the main function. The { and } braces indicate the start and end of the main function.

- fmt.Println(“Hello World”) - The Println function of the fmt package is used to write text to the standard output. package.Function() is the syntax to call a function in a package.

# What are packages and why are they used?

- A package is a collection of related Go files. 
- Packages are used to organize Go source code for better reusability and readability.

<!-- Packages are a collection of Go sources files that reside in the same directory. Packages provide code compartmentalization and hence it becomes easy to maintain Go projects. -->

# Go Module

- A Go Module is nothing but a collection of Go packages.

# Go command

- build : compile packages and dependencies
- run : compile and run Go program
- test : test packages
- fmt : gofmt (reformat) package sources
- mod : module maintenance

#  How does Go's garbage collector work? How do you reduce GC pressure?

Go's GC (garbage collector - automatic cleanup of memory your program no longer uses) runs alongside your program and pauses it for well under a
millisecond. It runs when the heap (memory in use) grows by about 100% since the last collection (tunable with GOGC), and GOMEMLIMIT sets a hard memory
ceiling.
Reduce pressure: fewer allocations > faster allocation. Reuse buffers (sync.Pool), preallocate slices/maps, avoid string concat in loops (strings.Builder), avoid
boxing hot values into interfaces, watch escape analysis (the compiler deciding if a value can stay in fast short-lived memory or must move to the heap; see it
with go build -gcflags=-m).
Your angle: "Under 10K concurrent users our hot path was per-frame/per-spin messages; we preallocated and pooled the hot structs so allocations per request stayed near zero
- GC pauses were never our p99 problem after that."
