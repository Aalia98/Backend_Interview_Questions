# 10. Goroutines and the Scheduler

### What is a goroutine? How is it different from an OS thread?
A goroutine is a lightweight unit of work managed by the Go runtime, started with the `go` keyword. It starts with a small stack (about 2 KB) that grows and shrinks automatically. The Go scheduler multiplexes many goroutines onto fewer OS threads. OS threads are heavier (about 1 MB stack, kernel scheduled).

### Why are goroutines lightweight?
Managed by the runtime, tiny growable stack, cheap creation and context switching, many goroutines share few OS threads. So programs can run hundreds of thousands of them.

### How many goroutines can Go create?
No fixed limit. It depends on memory (about 2 KB each at start). Hundreds of thousands or millions are possible depending on workload.

### Does every goroutine create an OS thread?
No. 10,000 goroutines may run on, for example, 8 OS threads through the scheduler.

### Does `go` guarantee parallel execution? Can goroutines run in parallel?
`go` gives concurrency. Parallelism happens when multiple CPU cores are available and `GOMAXPROCS` allows it. Otherwise goroutines take turns.

### Who schedules goroutines?
The Go runtime scheduler, not the OS. The OS schedules the OS threads.

### What happens when you write `go func(){...}()`?
A goroutine is created and placed in the scheduler's runnable queue, the caller returns immediately, and the scheduler decides when and on which thread it runs. Order is not guaranteed.

### Common wrong beliefs (true/false)
- A goroutine is an OS thread? No.
- Adding `go` guarantees parallel execution? No.
- `main()` waits for all goroutines? No.
- `time.Sleep` is enough for coordination? No.
- Goroutines run in the order they started? No.

### What is the Go scheduler?
Part of the runtime that maps many goroutines onto a few OS threads: scheduling, load balancing, work stealing, handling blocked goroutines, maximising CPU use.

### What is the GMP model?
- G = goroutine.
- M = machine, an OS thread.
- P = processor, a logical processor that holds a local run queue of goroutines. An M needs a P to run Go code.
Flow: G in the P's run queue, P attached to M, M runs on the CPU. There is also a global run queue. Idle Ps steal work from other Ps (work stealing). When a goroutine blocks in a syscall, its P is handed to another M so other goroutines keep running.

### What is GOMAXPROCS?
The maximum number of Ps, i.e. how many OS threads can execute Go code at the same time. Default is the number of CPU cores. Newer Go versions (1.25+) also respect container CPU limits; in older versions in Docker/Kubernetes people used the `automaxprocs` library.

### Is the scheduler preemptive?
Yes. Since Go 1.14 it is asynchronously preemptive, so a goroutine in a tight loop cannot block others forever.

### When should you use goroutines?
For independent or concurrent work: handling HTTP requests, external API calls, file I/O, background jobs, worker pools, queue processing, multiple DB queries. Avoid for tiny operations where overhead exceeds the benefit, or where shared mutable state is not synchronised.

### What is a goroutine leak? How do you avoid it?
A goroutine that never exits (blocked forever on a channel, missing cancellation). It keeps memory and can slowly crash the service. Avoid: always give goroutines a way to stop (`context` cancel, closing a channel, `done` channel), make sure senders/receivers are not left waiting. Detect with `pprof` goroutine profile or `goleak` in tests.

### What happens when main() returns while goroutines run?
The program exits immediately; other goroutines are killed.

### Exercises with answers
**Predict the output:** `go fmt.Println("A"); fmt.Println("B")`. Only B is guaranteed; main may exit before A prints.

**Why might "Task completed" never appear?** `main` can finish before the goroutine runs, and Go does not wait for goroutines. Use a WaitGroup or channel.

**`worker(1); worker(2)` vs `go worker(1); go worker(2)`:** the first runs one after another in the calling goroutine; the second runs concurrently, order and completion not guaranteed, so synchronisation is needed.

**Two goroutines printing "Backend" and "Engineer":** `time.Sleep` only experiments with scheduling; it does not guarantee completion or order. In production use a WaitGroup.