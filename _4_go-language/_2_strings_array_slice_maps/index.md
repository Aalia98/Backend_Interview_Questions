# 2. Strings, Arrays, Slices and Maps

### Array vs slice
An array has a fixed size (part of its type) and is copied on assignment. A slice is a dynamic, flexible view over an underlying array.

### What is a slice internally?
A small descriptor (slice header) with three fields: a pointer to the underlying array, `len`, and `cap`. The slice does not store the elements itself.

### len and cap
`len` is the number of elements in the slice. `cap` is the number of elements available from the slice's start to the end of the underlying array.

### What does `users[1:3]` mean?
For `users := []int{1,2,3,4}`, `users[1:3]` takes index 1 up to 3 (3 excluded), so the result is `[2 3]`. It shares the same underlying array.

### What does append do? What if capacity is exceeded?
```go
users = append(users, "Aalia")
```
If capacity is available, it writes into the existing array. If not, Go allocates a new, larger array, copies the elements, and returns a new slice pointing to it. That is why you must assign the result back.

### Slice capacity growth
When capacity is exceeded, Go allocates a bigger array and copies. Small slices roughly double; for large slices the growth is smaller (about 1.25x, plus a smoothing formula). Do not depend on the exact numbers. If you know the size, preallocate: `make([]T, 0, n)`.

### Passing a slice to a function
The slice header is copied, but it still points to the same underlying array. So changing elements inside the function is visible outside. But `append` inside the function may not be visible outside (the caller's `len` doesn't change) unless you return the new slice.

### Slice memory leak
A small sub-slice keeps the whole big underlying array alive, so memory cannot be freed. Fix: copy what you need into a new slice (`copy` or `slices.Clone`).

### How to copy a slice independently?
```go
dst := make([]int, len(src))
copy(dst, src)
// or: dst := slices.Clone(src)   // Go 1.21+
```
`b := a` only copies the header, both still share the same array.

### Nil slice vs empty slice
A nil slice (`var s []int`) has no underlying array, `s == nil` is true, `len` and `cap` are 0. An empty slice (`[]int{}` or `make([]int, 0)`) is not nil but also has length 0. `append`, `len` and `range` work on both. The difference shows up in `== nil` checks and in JSON: nil marshals to `null`, empty marshals to `[]`.

### String vs byte slice
A string is an immutable sequence of bytes, a `[]byte` is mutable. Strings are for text. `[]byte` is used when reading files, handling network data or JSON.

### How do strings and unicode work? len("é")?
A Go string holds UTF-8 bytes. `len(s)` is the number of bytes, not characters. `for i, r := range s` iterates by rune (code point). To count characters use `utf8.RuneCountInString(s)` or `len([]rune(s))`.

### Efficient string building
Strings are immutable, so `s += x` in a loop allocates every time. Use `strings.Builder`.

### What is a map?
A built-in key-value structure with fast lookup, insert and delete (average O(1)). Keys must be comparable (`==` works), so slices, maps and functions cannot be keys.

### Insert / delete / comma-ok
`m[3] = "John"`, `delete(m, 3)`. Comma-ok: `val, ok := m[key]` returns the value and a bool telling whether the key exists.

### What happens when a key doesn't exist? Why do we need ok?
You get the zero value of the value type. For a string map you get `""`, but you cannot tell whether the key is missing or the value is really `""`. `ok` tells you.

### Nil map. Can you write to it?
`var m map[int]string` is a nil map. Reading is fine (returns zero value). Writing panics. Initialise first: `m := make(map[int]string)`.

### Can you depend on map iteration order?
No. Go deliberately randomises the order. If you need order, collect and sort the keys.

### Are maps safe for concurrent use?
No. Concurrent reads are fine, but concurrent read+write or write+write is a data race (the runtime may crash with "concurrent map writes"). Protect with `sync.Mutex`/`sync.RWMutex`, or use `sync.Map` when its use case fits.

### When to use sync.Map instead of map + mutex?
Default to a normal map with a mutex (simpler, type-safe). `sync.Map` is for two cases: keys written once and read many times (like a cache that only grows), or many goroutines working on disjoint sets of keys.

### Why are slice and map called reference types?
They contain an internal pointer to data, so copies share the same underlying data. Precisely, Go is always pass-by-value: the slice header or map pointer is copied, but it points to shared data.

### Shallow copy vs deep copy
A shallow copy copies the value itself, but slices, maps and pointers inside still refer to the same data. A deep copy makes independent copies of everything needed. This matters when sharing data between goroutines or modifying cached objects.

### Synchronization (definition)
Synchronization means coordinating goroutines so they access shared data safely and in the right order, using mutexes, channels, WaitGroups or atomics.

### How do you check two slices or maps for equality?
`==` does not work on slices and maps (only comparison with `nil`). Use `slices.Equal`, `maps.Equal` (Go 1.21+) or `reflect.DeepEqual`.