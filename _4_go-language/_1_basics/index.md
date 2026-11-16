# 1. Go Basics

### What is Go?
- Go is an open-source, statically typed, compiled programming language developed by Google, designed for simplicity, high performance, and built-in concurrency using goroutines and channels.

### Why was Go created?
Google faced problems: slow compilation (C++), complex code (Java), poor concurrency support. So they built Go for fast compilation, easy concurrency (goroutines), simple syntax and high performance.

### Why use Go? / Advantages
- Fast performance: compiled to machine code, like C/C++.
- Built-in concurrency: goroutines are very lightweight.
- Simple syntax: less code, easier maintenance.
- Fast compilation: large projects compile in seconds.
- Strong standard library: HTTP server, JSON, files, concurrency tools.
- Garbage collected, so memory is managed automatically.
- Single static binary: easy to deploy (works well with Docker).

### Where is Go used?
Google, Uber, Netflix, Dropbox, Docker, Kubernetes (written in Go).

### Disadvantages of Go
- Error handling is verbose: `if err != nil { return err }` repeated many times.
- Generics came late (Go 1.18). They exist now, but are intentionally simpler than Java/C++ generics.
- Less expressive: no inheritance, no method overloading, no exceptions.
- The GC is good, but not suited for hard real-time work.

### Is Go object-oriented?
Go is not a classical OOP language (no classes, no inheritance), but it supports OOP ideas: encapsulation (exported/unexported names and methods on structs), polymorphism (interfaces), and code reuse through composition (struct embedding). Go prefers "composition over inheritance".

### What is a package? What is the main package?
- A package is a collection of Go files in the same directory. Packages organise code for reuse and readability.
- `package main` is special: it builds an executable, and the program starts from `func main()`.

### What is a Go module?
A module is a collection of packages that are versioned together. It is defined by a `go.mod` file (module path, Go version, dependencies) and `go.sum` (checksums of dependencies).

### What is init()?
`init()` runs automatically before `main()`, after package-level variables are initialised. A package can have many `init()` functions. You cannot call it yourself. Used for setup like registering drivers. Avoid heavy logic or hidden side effects in it.

### Hello world explained
```go
package main
import "fmt"
func main() {
    fmt.Println("Hello World")
}
```
- `package main`: entry-point package.
- `import "fmt"`: imports the formatting package.
- `func main()`: program execution starts here.
- `fmt.Println(...)`: writes to standard output. Syntax is `package.Function()`.

### Common go commands
- `go run` compile and run, `go build` compile to a binary, `go test` run tests, `go fmt` format code, `go mod init / tidy` manage modules, `go vet` find suspicious code, `go get` add a dependency.

### What happens if an import is unused?
It is a compile-time error. Go enforces clean code. (Same for unused local variables.)

### Zero value
The default value of a variable declared without initialisation: `0` for numbers, `""` for string, `false` for bool, `nil` for pointers, slices, maps, channels, functions and interfaces.

### `var` vs `:=`
`var` can be used at package level and inside functions, and can declare without a value (`var x int`). `:=` is short declaration, only inside functions, needs at least one new variable on the left, and infers the type.

### Constants and iota
- `const` values are fixed at compile time and cannot change.
- `iota` is a counter inside a `const` block. It starts at 0 and increases by 1 on each line.
```go
const (
    Pending = iota // 0
    Active         // 1
    Closed         // 2
)
```

### Type inference
The compiler decides the type from the assigned value: `x := 10` is `int`, `y := 3.14` is `float64`.

### What is a rune?
`rune` is an alias for `int32` and holds one Unicode code point. Use it for non-ASCII characters. `byte` is an alias for `uint8`.

### Data types in Go
`bool`, `int, int8, int16, int32, int64`, `uint, uint8 ... uint64`, `float32, float64`, `complex64, complex128`, `byte`, `rune`, `string`. Composite types: array, slice, map, struct, pointer, function, interface, channel.

### Scope
Block scope (each `{}`), function scope, package scope. An uppercase first letter means exported (visible outside the package).

### What is variable shadowing? Why is it dangerous? How to avoid?
Shadowing means creating a new variable with the same name in an inner scope, which hides the outer one. It is dangerous because of hidden bugs, especially with `err`:
```go
var err error
if x {
    result, err := doWork() // new err, outer err stays nil
    _ = result
}
```
Avoid: be careful with `:=` inside blocks, and use linters (`go vet -vettool` shadow, `golangci-lint`).

### Difference between `:=` and `=`
`:=` declares and assigns. `=` only assigns to an existing variable.

### How is Go different from Java?
No classes/inheritance (structs + interfaces + composition), no exceptions (errors are values), compiles to a native binary (no JVM), goroutines instead of heavy threads, implicit interfaces, simpler language with fewer features.

### Is Go compiled or interpreted? Is it statically typed?
Compiled to native machine code and statically typed (types are checked at compile time). It is also garbage collected.

### What is the blank identifier `_`?
It discards a value you don't need: `_, err := f()`. It is also used for side-effect imports: `import _ "github.com/go-sql-driver/mysql"` (runs that package's `init()`).

### What is `defer`? When does it run?
`defer` schedules a function call to run when the surrounding function is about to return (also on panic). Deferred calls run in LIFO order (last deferred, first run). Arguments are evaluated when the `defer` line runs, not when the call executes. Used for cleanup: close files, unlock mutexes, rollback transactions.
```go
f, err := os.Open("a.txt")
if err != nil { return err }
defer f.Close()
```
Gotcha: `defer` inside a loop runs only when the function ends, not each iteration.

### Loops and control flow in Go
Go has only `for` (as C-style loop, while-style, infinite, and `for range`). `switch` does not fall through by default (no `break` needed); use `fallthrough` to continue. `if` and `switch` can have an init statement: `if v, ok := m[k]; ok {}`.

### Loop variable capture in goroutines (Go 1.22 change)
Before Go 1.22, all iterations shared one loop variable, so goroutines/closures started in a loop often saw the last value. From Go 1.22, each iteration has its own variable. In older code, pass it as an argument: `go func(i int){...}(i)`.

### `goto` statement
`goto` jumps to a labeled statement in the same function. It is rarely used.
```go
fmt.Println("Start")
goto SkipAhead
fmt.Println("never printed")
SkipAhead:
fmt.Println("End")
```