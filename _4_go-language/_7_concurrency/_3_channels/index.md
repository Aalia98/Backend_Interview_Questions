# What is a channel in Go, and why do we use it?

- A channel is a way for goroutines to communicate with each other. We use it to transfer data from one goroutine to another and to synchronize their work when needed.

# What is the difference between a buffered channel and an unbuffered channel in Go?

- The main difference is that an unbuffered channel cannot store values, so the sender waits until a receiver is ready. A buffered channel can store a limited number of values, so the sender can send data without waiting for the receiver immediately. However, if the buffer becomes full, the sender has to wait.

# What happens when you send a value to an unbuffered channel but no goroutine is receiving from it?

- When we send a value to an unbuffered channel and no goroutine is receiving from it, the sender blocks and waits for a receiver. If no other goroutine can make progress, Go detects a deadlock and terminates the program.

# What is the purpose of close() in Go channels?

- The close() function indicates that no more values will be sent through the channel.

# What happens if you send a value to a channel after it has been closed?

- Sending a value to a closed channel causes a panic in Go.

# What happens when you receive from an empty, unbuffered channel and no goroutine is sending to it?

- When we receive from an empty, unbuffered channel and no goroutine is sending, the receiver blocks and waits for a value. If no goroutine can make progress, Go reports a deadlock. However, if the channel is closed and drained, receiving returns the zero value of the channel's type.

# Can multiple goroutines send and receive data through the same channel?

- Yes, multiple goroutines can send and receive data through the same channel. However, we need to be careful when closing the channel. We should close it only when no more values will be sent, because sending to a closed channel causes a panic.

# What is the difference between a channel and a sync.WaitGroup?

- A channel is used to communicate between goroutines and transfer data, while a WaitGroup is used to wait for goroutines to finish their tasks.

# Imagine you start three goroutines. Each one calculates a result, and the main goroutine needs to collect all three results before continuing. Would you use a channel, a WaitGroup, or both?

- I would use channels because the main goroutine needs to collect results from all three goroutines. The channels allow the goroutines to send their results to the main goroutine. I could also use a WaitGroup if I need to wait until all three goroutines finish their work.

# What is a select statement in Go, and why would you use it with channels?

- The select statement in Go allows us to handle multiple channel operations. It waits until one of the channel operations is ready and executes the corresponding case. We commonly use it when working with multiple channels or when handling timeouts and cancellation.

# Imagine you have two channels, ch1 and ch2. Both are waiting to receive data. Why would you use select instead of simply receiving from ch1 first and then ch2?

- The select statement allows us to wait for multiple channel operations. It executes the case whose channel operation is ready. If multiple cases are ready, Go chooses one of them pseudo-randomly. We commonly use select when working with multiple channels, timeouts, and cancellation.

# What is a race condition in Go? How can you prevent it?

- A race condition occurs when multiple goroutines access shared data concurrently and the result depends on their execution order. For example, if two goroutines increment the same counter without synchronization, an increment can be lost. We can prevent this using a mutex, atomic operations, or channels, depending on the use case.

# What is the difference between sync.Mutex and sync.RWMutex in Go?

- A mutex allows only one goroutine at a time to enter a protected section of code. Lock() acquires the lock, and Unlock() releases it so another goroutine can access the protected section.
WHERE AS
RWMutex allows multiple readers to access the data simultaneously. However, when a goroutine needs to write, it acquires an exclusive lock, preventing other readers and writers from accessing the protected data until the lock is released.

# What is a deadlock in Go, and can you give me one example of how it happens?

- A deadlock occurs when goroutines are blocked waiting for something that can never happen, so the program cannot make progress. For example, sending a value to an unbuffered channel without any receiver can cause a deadlock if all goroutines become blocked.

# What is the difference between == and reflect.DeepEqual() in Go? Can we compare two slices using ==?

- ==	reflect.DeepEqual()
Compares values for types that support ==	Checks deep equality of supported values
Cannot compare two slices directly	Can compare slices and their elements
Commonly used for comparable types	Useful for complex data structures, but has some special rules

# Suppose you have these two slices:
a := []int{10, 20, 30}
b := []int{10, 20, 30}

Can you use a == b to check whether they're equal? If not, what would you use instead?

- No, we cannot compare two slices directly using the == operator in Go. We can use reflect.DeepEqual() to check whether both slices contain deeply equal elements in the same order. Alternatively, for slices of comparable element types, we can use slices.Equal().

# What is the difference between a map and a sync.Map in Go? When would you use sync.Map?

- A regular map in Go is used to store key-value pairs, but it isn't safe for concurrent read/write access by multiple goroutines without synchronization. We can protect it using a mutex. sync.Map is concurrency-safe and provides methods like Store() and Load(). I would consider using sync.Map when the access pattern benefits from its concurrent operations, but I would use a regular map with a mutex when that is simpler for the application.

# Suppose three goroutines need to update the same regular map concurrently. What would you do to make it safe?

- I can make concurrent access to a regular map safe by protecting it with a sync.Mutex. Before updating the map, each goroutine acquires the lock and releases it after the operation. Alternatively, I can use sync.Map, which provides concurrency-safe operations such as Store() and Load().

# What is a nil channel in Go, and what happens if you try to send or receive data from it?

- nil channel hasn’t been initialized with make() and sending to or receiving from a nil channel blocks forever.

- A nil channel is different from a closed channel:
- Nil channel: sending or receiving blocks forever.
- Closed channel: sending causes a panic; receiving returns buffered values first, then the zero value.

# What is the difference between panic and error in Go?

- An error is a value that a function can return when something goes wrong. Usually, the program handles it and continues running.
WHERE AS
- A panic interrupts normal execution of the current goroutine. Go starts unwinding the function call stack, running deferred functions along the way. If the panic isn't recovered, the program terminates.

# What is defer in Go, and when does a deferred function execute?

- defer schedules a function call to execute when the surrounding function is about to return. It also runs when the function panics, as the stack unwinds.