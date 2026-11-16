# 13. Mutex, Atomic and Race Conditions

### What is a race condition (data race)? How do you detect and prevent it?
A data race: several goroutines access the same memory at the same time, at least one is a write, and there is no synchronisation. Results become unpredictable (for example lost increments). Detect with the race detector: `go run -race`, `go test -race`. Prevent with a mutex, atomic operations or channels.

### Buggy counter and the fix
`counter++` from 1000 goroutines is not guaranteed to print 1000: WaitGroup only waits, it does not protect data. Fix with a mutex:
```go
var mu sync.Mutex
go func() {
    defer wg.Done()
    mu.Lock()
    counter++
    mu.Unlock()
}()
```
or `atomic.AddInt64(&counter, 1)`.

### Mutex vs RWMutex
A Mutex lets one goroutine in at a time (`Lock`/`Unlock`). An RWMutex allows many readers at once (`RLock`/`RUnlock`), but a writer (`Lock`) needs exclusive access. Use RWMutex when reads far outnumber writes.

### Mutex vs sync/atomic
A Mutex protects a critical section (several related operations). Atomic does a single operation (add, load, store, compare-and-swap) on one value without locking, so it is faster for simple counters and flags. Choose by whether you need to protect a larger section or one simple operation.

### Mutex habits
Always `defer mu.Unlock()` right after `Lock()`. Do not copy a mutex (pass a pointer, keep it as a struct field). Keep the locked section small. A mutex is not re-entrant: locking twice in the same goroutine deadlocks.

### WaitGroup vs Mutex
WaitGroup: wait for goroutines to finish. Mutex: protect shared data. Different jobs.

### Two goroutines update the same order status. What do you use?
A mutex to protect the shared status (the second goroutine waits until the first unlocks). WaitGroup does not provide safety. Even better, make the database or a queue guarantee consistency (transactions, `SELECT ... FOR UPDATE`, optimistic locking with a version column), because in-memory locks don't work across several service instances.

### Data race vs deadlock
A data race is unsynchronised concurrent access to shared data (wrong results). A deadlock is when goroutines wait for each other forever (nothing progresses).

### What is a deadlock? How can it happen?
Goroutines blocked waiting for something that can never happen. Examples: send on an unbuffered channel without a receiver, locking the same mutex twice, two goroutines taking two locks in opposite order. Prevent by consistent lock ordering, timeouts/context, and avoiding holding locks while blocking.

### sync.Once
Runs a function exactly once even with many goroutines (lazy initialisation, singletons): `once.Do(func(){...})`.

### sync.Pool
A pool of reusable temporary objects to cut allocations and GC pressure (for example buffers). Items can be dropped by the GC any time, so never store state you need.

### `==` vs `reflect.DeepEqual`; can we compare two slices with `==`?
`==` works for comparable types; slices, maps and functions cannot be compared directly (only to nil). `reflect.DeepEqual` compares deeply, including slices and maps, but is slower and has special rules. For slices of comparable elements use `slices.Equal(a, b)` (Go 1.21+).

### Map vs sync.Map; three goroutines updating the same map
A normal map is unsafe for concurrent writes: protect it with `sync.Mutex` (or `RWMutex`). `sync.Map` is built in and safe (`Store`, `Load`), best for the read-mostly or disjoint-keys cases; otherwise a map plus a mutex is simpler and type-safe.