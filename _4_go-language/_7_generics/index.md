# 7. Generics

### What are generics in Go?
Generics let us write functions and data structures that work with different types without duplicating the logic for each type.
```go
func First[T any](items []T) (T, bool) {
    var zero T
    if len(items) == 0 { return zero, false }
    return items[0], true
}
```
`T` is a type parameter and `any` means it can be any type. Use generics for reusable utilities; do not use them when a normal function or interface is simpler.

### What are type constraints?
They limit which types a type parameter accepts. Examples: `comparable` (types that support `==`), `cmp.Ordered`, or a custom constraint:
```go
type Number interface { ~int | ~int64 | ~float64 }
func Sum[T Number](xs []T) T { var s T; for _, x := range xs { s += x }; return s }
```

### Generics vs interfaces: when to use which?
Interfaces: when behaviour differs per type (call methods). Generics: when the logic is the same but the type changes (containers, `Map`, `Filter`, `Max`). Since Go 1.18; also useful: `slices` and `maps` packages (1.21).