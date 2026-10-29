# What is a Process?

- A process is an independent running instance of a program with its own memory space and operating system resources.
Example
    - Chrome
    - VS Code
    - Spotify

# What is a Thread?

- A thread is the smallest unit of execution inside a process. Multiple threads share the same memory but execute independently.
Chrome may have:
    - UI Thread
    - Network Thread
    - Rendering Thread
    - JavaScript Thread

# Difference between Process and Thread?

- A process has its own memory and resources, while threads share the same process memory and are used for concurrent execution.

# What is the difference between concurrency and parallelism?

- Concurrency is about managing multiple tasks that can make progress during overlapping periods. For example, one chef can switch between several dishes. Parallelism means executing multiple tasks at the same instant, such as two chefs cooking different dishes simultaneously.

# What is the difference between sayHello() and go sayHello()?

- A normal function call executes synchronously in the calling goroutine. Adding the go keyword starts the function in a new goroutine, allowing the caller to continue without automatically waiting for that function to complete. Synchronization is needed when the caller must wait for the result or completion.

# In Example B, why did we use time.Sleep? Is it a reliable production solution?

- time.Sleep pauses the current goroutine for a specified duration. It can be useful in demonstrations or when a timed delay is genuinely required, but it should not be used to guarantee that another goroutine has finished. A WaitGroup or another appropriate synchronization mechanism should be used when completion must be coordinated.

# Can concurrency improve a backend service's responsiveness even if the machine has only one CPU core? Why?

- Yes. Concurrency can improve responsiveness on a single CPU core because the program can make progress on other tasks while one task waits for I/O, such as a database or network response. However, concurrency does not guarantee faster execution for CPU-bound workloads, and a single core cannot execute two instructions simultaneously.
