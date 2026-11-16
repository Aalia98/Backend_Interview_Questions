# 6. Interfaces

### What is an interface?
A collection of method signatures that defines behaviour: what a type can do, not how it does it. Different types can be used interchangeably if they implement the same methods.

### Why use interfaces?
They give loose coupling and polymorphism. Different types can satisfy the same interface, so implementations can be swapped easily, which helps testing (mocks) and dependency injection. Dependency injection means an object receives what it needs from outside instead of creating it.

### How is an interface implemented? Is there an `implements` keyword?
No keyword. Implementation is implicit: a type satisfies an interface when it has all the required methods.

### Interface vs concrete type / struct
A concrete type (struct) defines real data and behaviour. An interface defines the behaviour another type must provide. Example: `MySQLUserRepository` is concrete, `UserRepository` is the interface; the service depends on the interface, so it is easy to test or change the database.

### Empty interface
`interface{}` (alias `any` since Go 1.18) has no methods, so it can hold any value. Use only when the type is really unknown; you lose compile-time type safety.

### Type assertion vs type switch
A type assertion extracts a concrete type: `s, ok := v.(string)`. Always use the comma-ok form, otherwise it panics on a wrong type. A type switch handles several possible types:
```go
switch x := v.(type) {
case int:    fmt.Println("int", x)
case string: fmt.Println("string", x)
default:     fmt.Println("other")
}
```

### Interface embedding / composition
One interface can include other interfaces. The result requires all their methods plus its own, e.g. `io.ReadWriter` = `Reader` + `Writer`.

### What is a nil interface? The nil-interface gotcha
An interface value has two parts: (dynamic type, dynamic value). It is `== nil` only when both are nil. If you put a nil `*MyErr` pointer into an `error` variable, the interface is not nil (it has a type):
```go
func f() error {
    var e *MyErr = nil
    return e          // returns NON-nil error!
}
```
Fix: return a literal `nil` when there is no error.

### Can an interface hold a pointer?
Yes. Both values and pointers can be stored, as long as the method set matches (see method sets above). Interface holding a pointer means the method can modify the original.

### "Accept interfaces, return structs"
A Go proverb: functions should take the smallest interface they need as parameters (flexible and testable) and return concrete types (callers get full functionality). Define interfaces where they are used (consumer side) and keep them small (1 to 3 methods), like `io.Reader`.

### Some standard interfaces to know
`error` (`Error() string`), `fmt.Stringer` (`String() string`), `io.Reader`, `io.Writer`, `sort.Interface`, `http.Handler` (`ServeHTTP`), `context.Context`.