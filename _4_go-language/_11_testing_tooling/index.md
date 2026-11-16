# 17. Testing and Tooling

### How do you write tests in Go?
Files named `xxx_test.go`, functions `TestXxx(t *testing.T)`, run with `go test ./...`. Use `t.Errorf` (continue) or `t.Fatalf` (stop).

### What is a table-driven test?
A slice of test cases (name, input, expected) looped over with `t.Run`. It is the standard Go style.
```go
func TestAdd(t *testing.T) {
    tests := []struct{ name string; a, b, want int }{
        {"positive", 1, 2, 3},
        {"zero", 0, 0, 0},
    }
    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            if got := Add(tc.a, tc.b); got != tc.want {
                t.Errorf("got %d want %d", got, tc.want)
            }
        })
    }
}
```

### How do you mock dependencies?
Depend on an interface (for example `UserRepository`), pass a fake implementation in tests. Tools: hand-written fakes, `testify/mock`, `gomock`. Use `httptest` for handlers and `sqlmock` for the DB.

### Benchmarks, coverage, race tests
`func BenchmarkX(b *testing.B)` with `for i := 0; i < b.N; i++`; `go test -bench .`. Coverage: `go test -cover`. Race: `go test -race`.

### What is `go vet`, `gofmt`, linters?
`gofmt` formats code (one standard style). `go vet` finds suspicious code. `golangci-lint` runs many linters at once.