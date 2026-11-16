# 14. context

### What is context.Context and why use it?
It carries cancellation signals, deadlines/timeouts and request-scoped values across API boundaries and goroutines. In backends every request gets a context; when the client disconnects or the timeout hits, everything downstream (DB query, HTTP call, goroutines) can stop.

### Types of context
`context.Background()` (root, in main/tests), `context.TODO()` (placeholder), `WithCancel`, `WithTimeout`, `WithDeadline`, `WithValue`.
```go
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()
select {
case res := <-work(ctx): fmt.Println(res)
case <-ctx.Done():       fmt.Println(ctx.Err()) // deadline exceeded
}
```

### Rules for using context
Pass it as the first parameter named `ctx`. Do not store it in a struct. Always call `cancel()` (use `defer`) to free resources. Use `WithValue` only for request-scoped data (request ID), never for optional function parameters. Use `db.QueryContext(ctx, ...)` and `http.NewRequestWithContext`.

### How do you stop a goroutine?
Give it a `ctx.Done()` (or `done` channel) case in its loop:
```go
go func() {
    for {
        select {
        case <-ctx.Done(): return
        case job := <-jobs: process(job)
        }
    }
}()
```