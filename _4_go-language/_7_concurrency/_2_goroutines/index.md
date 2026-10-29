# A goroutine is an OS thread?

- A goroutine is a lightweight execution unit managed by the Go runtime; it is not the same as an OS thread.

# Adding go guarantees parallel execution?

- It enables concurrent execution. Actual parallel execution depends on available processing resources and runtime scheduling.

# main() waits for all goroutines automatically?

- It doesn't. Use an appropriate synchronization mechanism when work must finish before the program continues.

# time.Sleep is enough to coordinate goroutines?

- Sleep provides a delay, not a completion guarantee.

# Goroutines always run in the order they are started?

- Their execution order is not guaranteed without synchronization.

# Q. What is a Goroutine?

- A goroutine is a lightweight thread managed by the Go runtime for concurrent execution.
- Example
    go sendEmail()

# Q. How do you create a Goroutine?

- A goroutine is created by adding the go keyword before a function call.
- Example:
    go fetchData()
- The function starts executing concurrently.

# Q. Why are Goroutines Lightweight?

- Goroutines are lightweight because:
    They are managed by the Go runtime, not directly by the operating system.
    They start with a very small stack (about 2 KB).
    Their stack grows and shrinks automatically.
    The Go scheduler efficiently maps many goroutines onto fewer OS threads.
- Because of this, Go applications can create a very large number of goroutines efficiently.

# Q. How many Goroutines can Go create?

- There is no fixed limit.
- The number depends on:
    Available memory
    System resources
    Stack usage
- Since each goroutine starts with only about 2 KB of stack, Go programs can often create hundreds of thousands or even millions of goroutines, depending on workload and memory.
- There is no fixed limit. The practical limit depends on available memory and system resources.

# Q. Does every Goroutine create an OS Thread?

- No.
- Many goroutines are multiplexed onto a smaller number of OS threads by the Go scheduler.
- Example:
    10000 Goroutines
    ↓
    Go Scheduler
    ↓
    8 OS Threads
- This is one reason Go is efficient.
- No. Multiple goroutines share a smaller number of OS threads managed by the Go runtime.

# Q. Can Goroutines run in Parallel?

- Yes.
- If:
    The machine has multiple CPU cores.
    GOMAXPROCS allows multiple logical processors.
- Otherwise, goroutines still run concurrently by taking turns on available threads.
- Yes. Goroutines can run in parallel when multiple CPU cores are available and the scheduler is allowed to use them.

# Q. Who schedules Goroutines?

- The Go Scheduler schedules goroutines.
- It decides:
    Which goroutine runs
    When it runs
    Which OS thread it uses
- The operating system schedules OS threads, while the Go scheduler schedules goroutines.
- Goroutines are scheduled by the Go runtime scheduler, not directly by the operating system.

# Q. What happens when you write go func()?

- When you write:
    go func() {
        fmt.Println("Hello")
    }()
- Go:
    Creates a new goroutine.
    Places it into the scheduler's runnable queue.
    Returns immediately to the caller.
    The scheduler decides when and on which OS thread the goroutine executes.
- The execution order is not guaranteed.
- The go keyword creates a new goroutine and hands it to the Go scheduler for concurrent execution.

# Exercise 1 — Predict the output
func main() {
    go fmt.Println("A")
    fmt.Println("B")
}
What output is guaranteed? What might happen to "A"?

- Only B is guaranteed to be printed because it executes in the main goroutine. The A print statement runs in a separate goroutine, and main() does not automatically wait for it to finish. When main() returns, the program may exit before A is printed.

# Exercise 2 — Find the problem
func main() {
    go func() {
        fmt.Println("Task completed")
    }()
}
Why might "Task completed" never appear?

- The message might not appear because the main goroutine can finish before the new goroutine executes. Go does not automatically wait for all goroutines when main() returns. We can use synchronization, such as sync.WaitGroup or channels, to wait for the required work to complete.

# Exercise 3 — Explain the difference
worker(1)
worker(2)
versus
go worker(1)
go worker(2)
How does the execution behavior differ?

- Normal function calls execute synchronously in the calling goroutine, so the first call returns before the second begins. Adding the go keyword starts each function in a separate goroutine, allowing concurrent execution. The order and completion of these goroutines are not guaranteed, and synchronization is needed if the caller must wait for them.

# Exercise 4 — Write code
Write a small Go program that starts two goroutines. One prints "Backend" and the other prints "Engineer". For this exercise, use time.Sleep only to experiment with scheduling; explain why it isn't a reliable way to guarantee both messages are printed.

- package main

import (
    "fmt"
    "time"
)

func main() {
    go fmt.Println("Backend")
    go fmt.Println("Engineer")

    time.Sleep(time.Millisecond)
}
- The program starts two goroutines to print the messages. time.Sleep() gives them time to execute, but it does not guarantee that they have completed. The output order is also unpredictable. In production code, I would use synchronization, such as sync.WaitGroup, when I need to wait for both goroutines.

# What is sync.WaitGroup?

- A sync.WaitGroup waits for a collection of goroutines to finish their work.

# Q1. Predict the output
var wg sync.WaitGroup
wg.Add(1)
go func() {
    defer wg.Done()
    fmt.Println("A")
}()
wg.Wait()
fmt.Println("B")
Which output order is guaranteed, and why?

- The output order is guaranteed to be A followed by B. The wait group counter is incremented before starting the goroutine. After printing A, the deferred Done() call decrements the counter to zero. Wait() then returns, allowing the main goroutine to print B.

# Q2. Why do we call wg.Add(2) before starting the goroutines?

- I call Add(2) before starting the goroutines so the wait group counter reflects the expected work before Wait() is called. This prevents the caller from proceeding while the counter is still zero and the goroutines have not yet registered their tasks.

# Q3. What can go wrong if we forget wg.Done()?

- If we forget to call Done(), the wait group counter may never reach zero. Consequently, Wait() can block forever, preventing the caller from proceeding. Using defer wg.Done() at the beginning of the worker function helps ensure that the counter is decremented when the function returns.

# Q4. Coding exercise
Write a Go program that starts three goroutines. Each should print its worker ID, and main() must print "All done" only after all three workers finish. Use sync.WaitGroup, not time.Sleep().

- package main

import (
    "fmt"
    "sync"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()

    fmt.Println("Worker", id, "finished task")
}

func main() {
    var wg sync.WaitGroup

    wg.Add(3)

    go worker(1, &wg)
    go worker(2, &wg)
    go worker(3, &wg)

    wg.Wait()

    fmt.Println("All done")
}

# Q. What is the Go Scheduler?

- The Go Scheduler is part of the Go runtime.
- Its job is to efficiently execute goroutines by mapping many goroutines onto a smaller number of OS threads.
- Responsibilities:
    Scheduling goroutines
    Load balancing
    Work stealing
    Managing blocked goroutines
    Maximizing CPU utilization
- The Go Scheduler manages goroutines by efficiently scheduling them onto OS threads.

# Q. What is the GMP Model?

- The GMP model is Go's scheduling architecture.
- It consists of:
    G (Goroutine)
    Represents a goroutine.

    M (Machine)
    Represents an OS thread.

    P (Processor)
    Logical processor that schedules goroutines.
    Holds a local run queue of goroutines.
- Flow:
    G (Goroutine)
        ↓
    P (Processor)
        ↓
    M (OS Thread)
        ↓
    CPU
    Without a P, an M cannot execute goroutines.
- GMP is Go's scheduler model where Goroutines (G) are scheduled by Processors (P) and executed by OS Threads (M).

# Q. What is GOMAXPROCS?

- GOMAXPROCS specifies the maximum number of logical processors (P) that can execute Go code simultaneously.
- It controls the level of parallelism.
- Example:
    runtime.GOMAXPROCS(4)
    This allows Go to execute goroutines on up to four logical processors concurrently.
    By default, modern Go versions use the number of available CPU cores.
- GOMAXPROCS controls how many logical processors can execute Go code simultaneously, determining the maximum parallelism.

# Q. When should you use Goroutines?

- Use goroutines for tasks that can run independently or concurrently, such as:
    Handling HTTP requests
    Calling external APIs
    Reading or writing files
    Background jobs
    Worker pools
    Processing queues
    Parallel computations
    Multiple database queries
- Avoid using goroutines for:
    Tiny operations where the overhead outweighs the benefit
    Shared mutable state without proper synchronization
    Sequential tasks that don't benefit from concurrency
- Use goroutines when tasks can execute independently or concurrently and doing so improves responsiveness or throughput.

## --------------------------------------------------------------

## Topic 1: Concurrency and Goroutines
# Q1. What is concurrency in Go?

- Concurrency means handling multiple tasks during overlapping periods of time. A program can make progress on one task while another task is waiting. In Go, we mainly use goroutines and channels to build concurrent programs.

# Q2. What is the difference between concurrency and parallelism?

- Concurrency is about managing multiple tasks that can make progress independently. Parallelism means executing multiple tasks at exactly the same time. A concurrent program doesn't necessarily run in parallel.
For example, one chef can switch between several dishes is concurrency and two chefs cooking different dishes simultaneously is parallelism.

# Q3. What is a goroutine?

- A goroutine is a lightweight unit of execution managed by the Go runtime. We can start a goroutine by using the go keyword before a function call. Goroutines are cheaper to create than traditional OS threads, so Go can handle many of them.

# Q4. Does starting two goroutines guarantee parallel execution?

- No. Starting two goroutines allows their execution to overlap, but it doesn't guarantee that they run simultaneously. Actual parallel execution depends on scheduling and available CPU resources.

# Q5. What happens when the main() function returns while goroutines are still running?

- The Go program exits when main() returns. It doesn't automatically wait for other goroutines to finish, so their work may be interrupted.

## Topic 2: Go Scheduler
# Q6. What is the Go scheduler?

- The Go scheduler is responsible for scheduling goroutines to run on operating system threads. It helps Go to manage a large number of goroutines efficiently.

# Q7. What is the G-M-P model in Go?

- G stands for goroutine, M stands for machine or OS thread, and P stands for processor, which provides the resources needed to execute Go code. The scheduler uses these components to run goroutines efficiently.

# Q8. Can goroutines execute concurrently on a single CPU?

- Yes. The scheduler can switch between goroutines, allowing multiple tasks to make progress even on a single CPU. However, they won't execute at exactly the same instant on that single CPU.

## Topic 3: sync.WaitGroup
# Q9. What is sync.WaitGroup?

- A WaitGroup is used to wait for a collection of goroutines to finish their work. We increase its counter using Add(), decrease it using Done(), and call Wait() to block until the counter becomes zero.

# Q10. What is the difference between Add(), Done(), and Wait()?

- Add() increases or adjusts the counter, Done() decreases it by one, and Wait() blocks until the counter reaches zero.

# Q11. Why should we call wg.Add(1) before starting a goroutine?

- We should register the task before starting the goroutine. Otherwise, Wait() might see a zero counter and return before the goroutine calls Add().

# Q12. Why do we use defer wg.Done()?

- It ensures that Done() is called when the goroutine's function returns. This helps prevent the counter from being left positive accidentally.

# Q13. Can we pass a WaitGroup by value to a function?

- We should avoid copying a WaitGroup after it has started being used. Passing a pointer lets the function operate on the same WaitGroup as the caller.

# Q14. Does a WaitGroup prevent data races?

- No. A WaitGroup only helps us wait for goroutines to finish. If multiple goroutines access shared data concurrently, we may need a mutex or atomic operations to protect that data.

# Q15. Can we reuse a WaitGroup?

- Yes. We can reuse it for another batch after the previous batch has completed and Wait() has returned. We must follow the counter and synchronization rules.

# Q16. What happens if the WaitGroup counter becomes negative?

- Go panics with a negative WaitGroup counter error. This usually happens when Done() is called more times than the counter allows.