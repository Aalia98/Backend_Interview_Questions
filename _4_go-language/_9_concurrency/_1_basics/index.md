# 9. Concurrency: Basics

### What is a Process?

- A process is an independent running instance of a program with its own memory space and operating system resources.
Example
    - Chrome
    - VS Code
    - Spotify

### What is a Thread?

- A thread is the smallest unit of execution inside a process. Multiple threads share the same memory but execute independently.
Chrome may have:
    - UI Thread
    - Network Thread
    - Rendering Thread
    - JavaScript Thread

### Process vs thread
A process is an independent running program with its own memory and OS resources (Chrome, VS Code). A thread is the smallest unit of execution inside a process; threads of one process share memory but run independently.

### Concurrency vs parallelism
Concurrency is managing multiple tasks that can make progress during overlapping periods (one chef switching between dishes). Parallelism is executing multiple tasks at the same instant (two chefs cooking). A concurrent program is not necessarily parallel.

### `sayHello()` vs `go sayHello()`
A normal call runs synchronously in the calling goroutine. `go` starts it in a new goroutine and the caller continues immediately without waiting. If the caller must wait for completion or result, synchronisation is needed.

### Why is time.Sleep not a production solution?
`Sleep` only delays; it gives no guarantee that another goroutine has finished. Use WaitGroup, channels or another proper synchronisation.

### Can concurrency help on a single CPU core?
Yes, for I/O-bound work: while one task waits for DB or network, another progresses. For CPU-bound work it does not make things faster, and one core cannot run two instructions at the same instant.

### CPU-bound vs I/O-bound: what to use?
I/O-bound (API calls, DB, files): many goroutines help a lot. CPU-bound: useful parallelism is limited by cores, so use about `GOMAXPROCS` workers (a worker pool).