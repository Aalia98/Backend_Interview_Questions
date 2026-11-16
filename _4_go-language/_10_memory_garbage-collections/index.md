# 16. Memory and Garbage Collection

### How does Go's garbage collector work? How do you reduce GC pressure?
Go's GC (automatic cleanup of memory your program no longer uses) is a concurrent, tri-colour mark-and-sweep collector. It runs alongside your program, with only very short stop-the-world pauses (usually well under a millisecond). It starts a cycle when the heap grows by about 100% since the last collection (`GOGC=100`, tunable). `GOMEMLIMIT` sets a soft memory limit: the GC works harder as the program nears it (it is not a hard cap).

Reduce pressure: fewer allocations. Reuse buffers (`sync.Pool`), preallocate slices and maps, use `strings.Builder` instead of string concatenation in loops, avoid boxing hot values into interfaces, and check escape analysis with `go build -gcflags=-m`.

Your angle: "Under 10K concurrent users our hot path was per-frame/per-spin messages; we preallocated and pooled the hot structs so allocations per request stayed near zero - GC pauses were never our p99 problem after that."

### How do you find memory leaks or performance problems?
`pprof` (`net/http/pprof`): heap profile for memory, CPU profile, goroutine profile for leaks. Benchmarks: `go test -bench . -benchmem`. Trace: `go tool trace`.

### Common causes of memory leaks in Go
Goroutine leaks, sub-slices keeping big arrays, unbounded caches/maps, forgotten `time.Ticker.Stop()`, un-closed response bodies (`resp.Body.Close()`), not closing DB rows.