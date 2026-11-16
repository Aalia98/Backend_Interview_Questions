# 3. Functions

### Function vs method
A function is independent. A method is a function attached to a type through a receiver.

### Variadic function
A function whose last parameter is prefixed with `...` accepts any number of arguments of that type: `func sum(nums ...int)`. Inside, `nums` is a slice. Pass an existing slice with `sum(xs...)`.

### Anonymous functions and closures
An anonymous function has no name (function literal). A closure is a function value that captures variables from outside its body and can read and update them even after the outer function has returned.
```go
func counter() func() int {
    c := 0
    return func() int { c++; return c }
}
```

### Named return values
Return values given names in the signature. They start as zero values and a bare `return` returns them. Useful with `defer` to modify the returned error; overuse hurts readability.

### Call by value
Go is strictly call-by-value: a copy of every argument is passed. Changing the parameter does not change the caller's variable (unless it is a pointer, slice or map pointing to shared data).

### Can functions be passed as values?
Yes. Functions are first-class: assign to variables, pass as arguments, return from functions (used for middleware, options pattern, callbacks).

### Functional options pattern
A way to configure a struct with optional settings:
```go
type Option func(*Server)
func WithPort(p int) Option { return func(s *Server){ s.port = p } }
func NewServer(opts ...Option) *Server {
    s := &Server{port: 8080}
    for _, o := range opts { o(s) }
    return s
}
```