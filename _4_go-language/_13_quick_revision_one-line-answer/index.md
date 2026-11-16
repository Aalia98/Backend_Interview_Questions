# 19. Quick Revision: One-line Answers

- Is Go pass-by-reference? 
No, always by value.

- Can you write to a nil map? 
No, panic. Read? Yes.

- Send on closed channel? 
Panic. Receive? Zero value.

- Nil channel? 
Blocks forever.

- Does WaitGroup stop data races? 
No.

- Is map safe for concurrent writes? 
No.

- `defer` order? 
LIFO.

- Does `main` wait for goroutines? 
No.

- Slice header has? 
Pointer, len, cap.

- Interface holding nil pointer is nil? 
No.

- `errors.Is` vs `As`? 
Is = value match, As = type match.

- `make` vs `new`? 
make: slice/map/chan; 
new: pointer to zero value.

- Unbuffered vs buffered? 
Handshake vs store up to cap.

- Who closes a channel? 
The sender.

- Go GC type? 
Concurrent tri-colour mark-and-sweep.

- Is `GOMEMLIMIT` hard? 
No, soft.

- Does Go have exceptions? 
No: errors as values, panic only for bugs.

- Can goroutine panic be recovered by another? 
No.