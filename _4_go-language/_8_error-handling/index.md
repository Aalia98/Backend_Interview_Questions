# 8. Error Handling

### How does Go handle errors? Why check `err != nil`?
Go uses errors as values, not exceptions. Functions return a result plus an error. `nil` means no error.
```go
user, err := GetUser(id)
if err != nil { return err }
```
On success: value + nil. On failure: zero value + error.

### errors.New and fmt.Errorf
`errors.New("user not found")` creates a simple error. `fmt.Errorf("user %d not found", id)` makes a formatted error and supports wrapping with `%w`.

### What is %w? Why wrap errors?
`%w` in `fmt.Errorf` wraps an existing error and keeps the original in the error chain. We add context at each layer without losing the original, and later inspect it with `errors.Is()` or `errors.As()`. Example: the repository returns "not found", the service wraps it, and the handler still detects it and returns HTTP 404.
```go
return fmt.Errorf("get user %d: %w", id, err)
```

### %v vs %w
`%v` only formats the error text (the chain is lost). `%w` wraps, so `errors.Is` / `errors.As` still work.

### errors.Is vs errors.As
`errors.Is` checks if any error in the chain equals a particular error value (e.g. `sql.ErrNoRows`). `errors.As` checks if any error in the chain has a given type and assigns it, so you can read its fields.
```go
if errors.Is(err, sql.ErrNoRows) { ... }
var ve *ValidationError
if errors.As(err, &ve) { fmt.Println(ve.Field) }
```

### Custom error type; why implement Error() string?
Create a struct that implements `Error() string`. It then satisfies the built-in `error` interface and can carry extra info like a code or the resource name.
```go
type NotFoundError struct{ ID int }
func (e *NotFoundError) Error() string { return fmt.Sprintf("id %d not found", e.ID) }
```

### Sentinel errors
Package-level error values like `var ErrNotFound = errors.New("not found")`, compared with `errors.Is`. Name them `ErrXxx`.

### Error vs panic. Should I panic on a DB failure?
Errors are values for expected or recoverable failures. Panic is for situations where normal execution cannot continue (programmer bugs, impossible states). On a DB failure: no, return the error and let the proper layer handle it.

### defer, panic and recover
`defer` schedules a call to run when the function returns (cleanup). `panic` stops normal execution of the current goroutine and unwinds the stack running deferred calls. `recover` stops a panic, but only when called inside a deferred function in the same goroutine. Use `recover` at a deliberate boundary, for example middleware that stops one bad request from crashing the whole server, then log it.
```go
defer func() {
    if r := recover(); r != nil { log.Println("recovered:", r) }
}()
```

### Can a panic in one goroutine be recovered by another goroutine?
No. An unrecovered panic in any goroutine crashes the whole program. Each goroutine must recover its own panic.

### Common causes of runtime panic
Nil pointer dereference, index out of range, write to nil map, send on closed channel, close of closed channel, failed type assertion without comma-ok, negative WaitGroup counter, division by zero (integers).

### Where should errors be logged?
Handle an error once: either return it (with context) or log it, not both at every layer. Log at the top (handler/main).