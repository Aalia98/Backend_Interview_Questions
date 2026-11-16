# 4. Pointers and Memory

### What is a pointer? What do & and * do?
A pointer stores the memory address of another variable. `&x` gives the address of `x`. `*p` in a type (`*int`) declares a pointer type; as an operator it dereferences (goes to the address and reads/writes the actual value).

### When should you use pointers? / Why not always?
Use pointers to modify the original value, share data across functions, avoid copying large structs, or represent "optional" (nil). For small, immutable-like values passing by value is simpler and often faster.

### Value vs pointer. `update(user User)` vs `update(user *User)`
Go is pass-by-value. With `User` the whole struct is copied, so changes don't affect the original. With `*User` the pointer is copied, but both pointers refer to the same struct, so changes affect the original.

### Does Go pass pointers by reference?
No. Go has no pass-by-reference. The pointer value (address) is copied.

### Cheat sheet
```go
type User struct { Name string; Age int }
user := User{}      // value
p := &user          // pointer to struct
var q *User         // nil pointer
*p                  // dereference
p.Name              // same as (*p).Name
u2 := &User{}       // pointer to new struct
```

### Stack vs heap in Go
Stack: fast, per-goroutine, freed automatically when the function returns. Heap: shared, needs the garbage collector, slower. You don't choose manually; the compiler decides using escape analysis.

### What is escape analysis? How do you inspect it?
The compiler checks whether a variable can safely stay on the stack, or must "escape" to the heap (for example, when its address is returned or stored somewhere that outlives the function). Inspect with `go build -gcflags="-m"`.

### Can Go return a pointer to a local variable?
Yes. It is safe because escape analysis moves that variable to the heap.
```go
func newUser() *User { u := User{}; return &u }
```

### What is a nil pointer? What happens if you dereference it?
A pointer with no address (zero value of pointer types). Dereferencing it causes a runtime panic: `invalid memory address or nil pointer dereference`. Always check `if p != nil` before use, especially for values returned with errors.

### make vs new
`new(T)` allocates zeroed memory for type T and returns `*T`. `make` is only for slices, maps and channels and returns an initialised (ready to use) value of that type, not a pointer.

### Does Go have pointer arithmetic?
No (only through the `unsafe` package, which should be avoided).