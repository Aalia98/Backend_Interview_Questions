# 15. Concurrency Patterns

### Worker pool
A fixed number of goroutines read jobs from a channel, so you limit concurrency.
```go
jobs := make(chan int, 100)
results := make(chan int, 100)
var wg sync.WaitGroup
for w := 0; w < 5; w++ {
    wg.Add(1)
    go func() {
        defer wg.Done()
        for j := range jobs { results <- j * 2 }
    }()
}
for i := 0; i < 20; i++ { jobs <- i }
close(jobs)
wg.Wait()
close(results)
```

### Fan-out / fan-in
Fan-out: several goroutines read from one channel to split work. Fan-in: merge results from many channels into one.

### Limiting concurrency with a semaphore
A buffered channel as a semaphore: `sem := make(chan struct{}, 10)`; `sem <- struct{}{}` to acquire and `<-sem` to release. Prevents starting thousands of goroutines against a DB.

### errgroup
`golang.org/x/sync/errgroup` runs goroutines, waits for all, returns the first error and can cancel the others via context. Good for parallel API/DB calls in a handler.

### Rate limiting and timeouts
`time.Ticker` for steady rate, `golang.org/x/time/rate` token bucket, `context.WithTimeout` for deadlines on every external call.

### Real-world answer: where did you use concurrency?
Pattern for your answer: say the problem, the tool, and the safety measure. Example: "In my backend, one request had to call several services. I ran the calls in goroutines with a WaitGroup/errgroup, passed the request context so it cancels on timeout, and limited the count with a worker pool or semaphore. I ran tests with `-race`." Use only what you really did.