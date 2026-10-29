# What is a pointer?

- A pointer stores the memory address of another variable.
- It allows direct access and modification of the original value.

# When should you use pointers?

- Pointers are used when we need to modify the original value, share data across functions, or avoid copying large data structures.

# What does & do?

- It returns the address of a variable.

# What does * do?

- In a pointer type, it declares a pointer.
- When used as an operator, it dereferences the pointer and accesses the value it points to.

    Pointer = address
    Dereference = address par jaakar actual value lena

# Value vs pointer?

- Value: Represents the actual data itself. When passed, Go creates an exact copy of the data.

- Pointer: Represents the memory address of where the actual data lives. When passed, only the address is copied, not the data itself.

# Why would you pass a pointer to a struct instead of passing the struct itself?

- Passing a pointer allows the function to work with the original struct rather than a copy. 
- It allows modifications made inside the function to affect the original value. 
- It can also avoid copying a large struct.

# Go passes pointers by reference?

- Go uses pass-by-value. When we pass a pointer, the pointer value itself is copied, but both the original pointer and the copied pointer refer to the same underlying data.

# Why Not Always Use Pointers?

- I use a pointer when I need mutation, want to avoid copying a sufficiently large value, or need a nil/optional representation. For small immutable-like values, passing the value can be simpler.

# What is the difference between:

   - func update(user User)
     and:
   - func update(user *User)

- Go is pass-by-value. When we pass a struct, the struct itself is copied. When we pass a pointer to a struct, the pointer is copied, but both the original pointer and the copied pointer refer to the same underlying struct, so modifications through the pointer affect the original struct.

# Stack vs heap in Go?

Stack memory is fast and used for short-lived local variables, while heap memory is used for data that needs to survive beyond a function call or be shared.

# What is escape analysis?

Escape analysis is a process performed by the Go compiler to determine whether a variable can safely stay on the stack or needs to be moved to the heap.

# What is a nil pointer?
# What happens if we dereference a nil pointer?
# Can Go return a pointer to a local variable?
# How can you inspect escape-analysis decisions?

# CHEAT SHEET - - - - - - - - -

- STRUCT
    type User struct {
        Name string
        Age  int
    }

- VALUE
    user := User{}

- POINTER TO STRUCT
    p := &user

- POINTER TYPE
    var p *User

- ADDRESS
    &user

- DEREFERENCE
    *p

- STRUCT FIELD THROUGH POINTER
    p.Name

- EQUIVALENT
    (*p).Name

- POINTER STRUCT DIRECTLY
    user := &User{}

- NIL POINTER
    var user *User

## --------------------------- Slice ------------------------------

# Why slice and map are reference types?

Slices and maps internally store pointers to underlying data structures, so when they are passed around, changes affect the same underlying data.

# Array vs Slice ?

- An array has a fixed size determined at compile time, while a slice is a dynamically sized, flexible view into an underlying array

# What is a slice?

- A slice is a descriptor that provides a flexible view over an underlying array.

# What are len and cap?

- len is the number of elements currently in the slice.
- cap represents the capacity available from the slice's starting position in its underlying array.

# What does append do?

- users = append(users, "Aalia")

    If capacity is available:

        Existing underlying array
                ↓
        append element

    If capacity isn't sufficient:

        Old array
        ↓
        allocate new array
        ↓
        copy elements
        ↓
        append new element

That's why you should assign the result:
users = append(users, "Aalia")

# What happens when append exceeds capacity?

- Go allocates a new underlying array, copies the existing elements, and returns a new slice referencing it.

# What does users[1:3] mean?

- For example :- 
    users := []int{1,2,3,4}
    when I write users[1:3], it means I want values from index 1 to 3 (excluded). So, output will be 2,3.

# What is the internal representation of a slice?

- Internally, a Go slice is a descriptor containing three pieces of information: a pointer to the underlying array, the current length, and the capacity. The slice itself doesn't store all the elements; it references the underlying array.

# How do you copy a slice independently?

## ----------------------- Map -----------------------

# What is a map?

- A map is a built-in key-value data structure that provides efficient lookup, insertion, and deletion by key.

# How do you insert/delete?

- INSERT - users[3] = "John"
- DELETE - delete(users, 3)

# What is comma-ok?

- The "comma-ok" idiom in Go is a multi-value assignment syntax (value, ok := expression) that returns both the result of an operation and a boolean flag indicating whether it succeeded.

# What happens when a key doesn't exist?

- The map returns the zero value of its value type. The comma-ok form can be used to determine whether the key actually exists or not.

# Why do we need ok?

- If the value type is string, you'll get: ""

But is that because:
    key doesn't exist

or because:
    key exists and its value is ""

You use ok to distinguish them.

# What is a nil map?

- var users map[int]string
users is nil.

You can read:
fmt.Println(users[1])
That's okay.

But:

users[1] = "Aalia"
causes a runtime panic.

Initialize first:
users := make(map[int]string)

# Can you write to a nil map?

- No. Writing to a nil map causes a runtime panic. Reading from a nil map is allowed.

# Are Go maps safe for concurrent access?

- No, a regular Go map is not safe for concurrent access when there is concurrent mutation. Multiple concurrent reads are okay as long as the map isn't being modified, but concurrent reads and writes or multiple writes require synchronization. We can protect a map using sync.Mutex or sync.RWMutex, or use sync.Map when its use cases fit.

# What is the difference between a shallow copy and a deep copy?

A shallow copy copies the immediate value, but reference-like fields such as slices, maps and pointers may still refer to the same underlying data. A deep copy creates independent copies of the data that needs to be separated.

For example, copying a slice variable does not automatically copy all its elements into an independent backing array. If two slices share that array, modifying a shared element can affect both.

This matters when passing data between goroutines or modifying cached objects, because shared mutable data can introduce unexpected behavior.

# Synchronization means the act of making two or more things happen, move, or match up at the exact same time.

# When would you use sync.Map instead of a normal map with a mutex?
# Can you depend on map iteration order?