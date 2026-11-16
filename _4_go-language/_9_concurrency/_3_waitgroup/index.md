# 11. sync.WaitGroup

### What is sync.WaitGroup? Add, Done, Wait
It waits for a collection of goroutines to finish. `Add(n)` raises the counter, `Done()` lowers it by one, `Wait()` blocks until it reaches zero.

### Why call `wg.Add` before starting the goroutine?
So the counter is registered before `Wait()` can run. If `Add` were called inside the goroutine, `Wait()` might see a zero counter and return before the goroutine has called `Add`.

### Why `defer wg.Done()`? What if you forget Done?
`defer` guarantees `Done()` runs even on early return or panic. If `Done()` is forgotten, the counter never reaches zero, `Wait()` blocks forever and the program hangs (Go may report "all goroutines are asleep - deadlock!" if nothing else runs).

### What if Done is called more times than Add?
The counter goes negative and Go panics: `sync: negative WaitGroup counter`.

### Can we pass a WaitGroup by value? Can we reuse it?
Do not copy it after use; pass a pointer (`*sync.WaitGroup`). A copy has its own counter, so the original never gets `Done()`. It can be reused for another batch after `Wait()` has returned.

### Does a WaitGroup prevent data races?
No. It only waits. Shared data still needs a mutex or atomic.

### Program: print a message and wait with WaitGroup
```go
func printMessage(wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Println("Backend Engineer (Golang)")
}
func main() {
    var wg sync.WaitGroup
    wg.Add(1)
    go printMessage(&wg)
    wg.Wait()
    fmt.Println("Message print successfully")
}
```

### Program: three workers, then "All done"
```go
func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Println("Worker", id, "finished task")
}
func main() {
    var wg sync.WaitGroup
    wg.Add(3)
    for i := 1; i <= 3; i++ { go worker(i, &wg) }
    wg.Wait()
    fmt.Println("All done")
}
```

### Newer API: `wg.Go()`
Go 1.25 added `wg.Go(func(){...})` which does `Add(1)` and `Done()` for you. Know the classic way first; it is what interviewers ask.