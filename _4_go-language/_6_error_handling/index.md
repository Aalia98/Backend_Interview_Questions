# How does Go handle errors?

- Go generally uses errors as values instead of exceptions for ordinary error handling.
Example:
    user, err := GetUser(id)

    if err != nil {
        return err
    }
Think: Function return result + error

If successful: user + nil
If failed: zero value + error

# Why do we check err != nil?

Because nil means: No error

Example:
user, err := GetUser(id)

if err != nil {
    return err
}

If: err == nil -> execution continues.

# What is errors.New?

Used to create a simple error.

err := errors.New("user not found")
Now err contains an error value.

# What is fmt.Errorf?

It lets you create formatted errors.
err := fmt.Errorf("user %d not found", id)

It also supports wrapping:
fmt.Errorf("failed to get user: %w", err)

# What is %w or Why should errors be wrapped?

In Go, `%w` is used with `fmt.Errorf()` to wrap an existing error while preserving the original error in the error chain.

We use it to add meaningful context at different layers of an application without losing the original error. This allows us to inspect the underlying error later using `errors.Is()` or `errors.As()`.

For example, if the repository returns a user-not-found error, the service can wrap it with additional context using `%w`, and the handler can still identify the original error and return the appropriate HTTP status code.

# %v vs %w?

%v formats an error as a value, while %w wraps an error and preserves the underlying error for errors.Is and errors.As.

# Error vs Panic?

Errors are values used to communicate expected or recoverable failures. Panic is for exceptional situations where normal execution cannot continue and should not be used for ordinary application errors.

# Should I panic when database query fails?

Generally No.
Bad approach:
user, err := getUser(id)

if err != nil {
    panic(err)
}

In a backend service, you'd normally return the error and let the appropriate layer handle it.

# What is the difference between errors.Is and errors.As?

`errors.Is` checks whether an error matches a particular error value, including errors wrapped with additional context. `errors.As` checks whether an error in the chain can be assigned to a particular error type.
For example, I can use `errors.Is` to check for sql.ErrNoRows. I can use `errors.As` to inspect a custom error type that contains additional information about the failure.

# What is the difference between defer, panic and recover?

- `defer` schedules a function call to run when the surrounding function returns. It is useful for cleanup, such as closing a file or releasing a resource.

- `panic` stops normal execution of the current goroutine and begins unwinding the stack. recover can intercept a panic when called from a deferred function in the same goroutine.

- I would use `recover` at a deliberate boundary, such as protecting a server from a panic in an individual request handler, and log the failure. I would not use panic for ordinary business errors.

# How do you define a custom error type?

- In Go, we can define a custom error type by creating a struct and implementing the Error() string method. This allows us to include additional information in an error, such as an error code or the resource that caused the failure. We can then use this information to handle specific errors differently.

# Why implement Error() string?

Because Go's built-in error interface requires a type to implement the Error() string method. Once our custom type implements that method, it satisfies the error interface.