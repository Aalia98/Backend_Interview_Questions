# 5. Structs and Methods

### What is a struct? Why use it? Struct vs class?
A struct is a user-defined type that groups related fields (User, Order, Product). Go has no classes; it uses structs for data, and methods attached to types for behaviour.

### How do you initialise a struct? Uninitialised fields?
```go
user := User{ID: 1, Name: "Aalia", Email: "aalia@example.com"}
```
Fields you leave out get their zero values.

### JSON tags
Tags control the JSON field name, omit empty fields or skip fields: `json:"name,omitempty"`, `json:"-"` skips the field. Only exported (capitalised) fields are encoded.

### Struct embedding
Embedding puts one struct in another without a field name, so its fields and methods are promoted and directly accessible. It is how Go does composition.
```go
type Car struct { Engine; Model string }
c := Car{Engine: Engine{Horsepower: 300}, Model: "S"}
fmt.Println(c.Horsepower)
```

### Value receiver vs pointer receiver
A value receiver works on a copy. A pointer receiver works on the original, so it can modify the struct and avoids copying large structs. Rule of thumb: if any method needs a pointer receiver, use pointer receivers for all methods of that type.

### What is a method receiver?
The value a method operates on, written before the method name: `func (u *User) Save() {}`.

### Can you compare structs?
Yes with `==`, if all fields are comparable. A struct with a slice or map field cannot be compared with `==`.

### What is an anonymous struct?
A struct without a named type, used for one-off data (tests, JSON): `p := struct{ Name string }{"A"}`.

### Exported vs unexported fields
Names starting with uppercase are exported (visible to other packages); lowercase are private to the package. This is Go's encapsulation.

### Method sets and addressability (interview trap)
Value type `T` has only value-receiver methods in its method set. `*T` has both. So if `Save()` has a pointer receiver, then `T` does not implement an interface that needs `Save()`; only `*T` does.