# 12. Channels and select

### What is a channel and why do we use it?
A typed pipe for goroutines to send data to each other and synchronise their work. Go's idea: "Don't communicate by sharing memory; share memory by communicating."

### Buffered vs unbuffered channel
Unbuffered (`make(chan int)`) stores nothing: the sender blocks until a receiver is ready (a handshake). Buffered (`make(chan int, 3)`) stores up to its capacity: the sender blocks only when the buffer is full, the receiver blocks only when it is empty.

### Send on an unbuffered channel with no receiver / receive with no sender
The goroutine blocks. If every goroutine in the program is blocked, the runtime stops with `fatal error: all goroutines are asleep - deadlock!`. If other goroutines are still running, no error is shown and the blocked one just waits (a leak).

### What does close() do?
It says no more values will be sent. Receivers can still get the remaining buffered values, then get the zero value. Use `v, ok := <-ch`; `ok` is false when the channel is closed and drained.

### Sending to a closed channel / closing twice / closing nil
Send on a closed channel: panic. Close an already closed channel: panic. Close a nil channel: panic. Receiving from a closed channel is fine (zero value).

### Who should close a channel?
The sender (producer), never the receiver, and only once. With many senders, use a coordinator (WaitGroup + one goroutine that closes after `wg.Wait()`).

### Nil channel vs closed channel
A nil channel (not created with `make`): send and receive block forever. A closed channel: sending panics, receiving returns buffered values first and then zero values. (A nil channel inside `select` is a trick to disable a case.)

### Can multiple goroutines send/receive on the same channel?
Yes. Be careful when closing: close only when no more sends can happen.

### Channel direction
`chan<- int` send-only, `<-chan int` receive-only. Use them in function signatures to make intent clear and let the compiler catch mistakes.

### Channel vs WaitGroup
A channel transfers data and can synchronise. A WaitGroup only waits for goroutines to finish.

### Three goroutines each compute a result; main collects all three. Channel, WaitGroup, or both?
A channel is enough: results go through a buffered channel of size 3 and main receives three times (each receive waits for a result, so no WaitGroup needed).
```go
ch := make(chan int, 3)
for i := 0; i < 3; i++ {
    go func(n int) { ch <- n * n }(i)
}
for i := 0; i < 3; i++ { fmt.Println(<-ch) }
```
Use WaitGroup + channel together when the number of results is not known in advance: a goroutine does `wg.Wait(); close(ch)` and main does `for v := range ch`.

### What is select? Why use it instead of receiving from ch1 then ch2?
`select` waits on several channel operations and runs the case that is ready. If receiving from `ch1` first, you would block there even if `ch2` already has data. If several cases are ready, one is chosen pseudo-randomly. Used for multiplexing, timeouts and cancellation.
```go
select {
case v := <-ch1: fmt.Println(v)
case v := <-ch2: fmt.Println(v)
case <-time.After(2 * time.Second): fmt.Println("timeout")
default: // runs if nothing is ready (non-blocking)
}
```

### How do you range over a channel?
`for v := range ch { ... }` runs until the channel is closed. If nobody closes it, the loop blocks forever.

### What is the `done` channel / quit pattern?
A channel used only to signal cancellation. Closing it wakes up everyone listening on `<-done`. `context.Context` is the standard version of this.