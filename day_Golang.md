# What is a goroutine in Go, and how is it different from an operating-system thread?

- A goroutine is a lightweight unit of work managed by the Go runtime. It's not an OS thread—it starts with a small stack, grows if needed, and the Go scheduler multiplexes many goroutines onto fewer OS threads.

# If two goroutines want to share data, what are two safe ways to handle that in Go?

- Two common safe ways are channels for communication and mutex to protect shared data.

# What’s the purpose of the sync package in Go, and name one thing you’d use from it?

- The sync package provides tools like WaitGroup and mutex to coordinate goroutines and protect shared data from race conditions. For example, I’d use a WaitGroup to wait for multiple goroutines to finish or a mutex to safely update a counter.

# Write a goroutine that prints a message, and use WaitGroup to wait for it properly.

package main
import (
	"fmt"
	"sync"
)

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

# What happens when we forget to write wg.Done() in a program?

- If we forget to call `wg.Done()`, So, the WaitGroup counter will never reach zero. Consequently, `wg.Wait()` will wait indefinitely, meaning the program will get stuck at that point. This will appear as a deadlock, since the wait will never conclude, and the `main` function will not proceed.

# What happens if we call wg.Done() more times than wg.Add() has incremented the counter?

- If wg.Done() is called more times than Add(), the WaitGroup counter goes negative, and Go panics with a negative WaitGroup counter error.

# What’s the difference between sync.WaitGroup and sync.Mutex in Go?

- I use a WaitGroup when I need to wait for multiple goroutines to finish. I use a mutex when multiple goroutines access or update shared data, to avoid race conditions.
- for example: If I'm incrementing a counter from different goroutines, I would use a mutex to lock the counter.

# If two goroutines update the same order status, what you used WaitGroup or mutex?

- If two goroutines update the same order status, I would not use WaitGroup for safety. I would use a mutex, or better, design it so the database or a queue ensures consistency.

# If two goroutines are updating the same order, what would be the safe approach?

- I would use a mutex to protect the shared order status. When one goroutine acquires the lock, another goroutine must wait before entering the same critical section. Once the first goroutine releases the lock, the other can proceed. This helps prevent race conditions when accessing shared data.

# What is a race condition in Go, and how can you detect it?

- A data race occurs when multiple goroutines access the same memory concurrently, at least one access is a write, and the accesses are not properly synchronised. It can cause unpredictable behaviour. I can detect data races using Go’s built-in race detector with go run -race or go test -race.

# What is wrong with this code, and how would you fix it?
package main

import (
    "fmt"
    "sync"
)

func main() {
    counter := 0
    var wg sync.WaitGroup

    for i := 0; i < 1000; i++ {
        wg.Add(1)

        go func() {
            defer wg.Done()
            counter++
        }()
    }

    wg.Wait()
    fmt.Println(counter)
}

- No, it is not guaranteed to print 1000. The WaitGroup only waits for all goroutines to finish; it does not protect shared data. Since multiple goroutines increment the same counter without synchronization, a data race can occur. I would fix this by protecting the counter with a mutex or by using an atomic operation.

# What is the difference between a data race and a deadlock in Go?

- A data race happens when two goroutines access and modify shared data concurrently without proper synchronization, which can cause unpredictable results. A deadlock is when two goroutines wait on each other forever, so nothing progresses.

# What is the difference between sync.Mutex and sync/atomic in Go? When would you use each?

- A Mutex protects a critical section of code and allows only one goroutine at a time to access it. I use it when I need to protect shared data or multiple related operations. Atomic is useful for simple operations like incrementing a counter safely without using a mutex. I choose based on whether I need to protect a larger section of code or perform a specific atomic operation.