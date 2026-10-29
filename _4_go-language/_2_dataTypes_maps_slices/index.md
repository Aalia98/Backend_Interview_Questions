# What is iota?

- iota is a special identifier in Go used inside const blocks.
- It is automatically incremented integer counter.
- Starts from 0 and increases by 1 for each line in const block.

# Scope

- Scope determines variable accessibility from universe to block level.
- Types of Scope in Go

1. Block Scope (Most Common)
   - Each {} creates a new scope

2. Package Scope (Global)
   - accessible everywhere in package

3. Function Scope
   - Variables declared inside a function → used only inside that function

# What is variable shadowing?

- Shadowing is creating a new variable with same name in inner scope.
- It hides (shadows) outer variable

# Difference between := and =?

- := → declare + assign
- = → assign only

# Why shadowing is dangerous?

- Causes hidden bugs
- especially with err

# How to avoid it?

- careful with :=
- use linters

# data types available in Go

- bool
- int8, int16, int32, int64, int
- uint8, uint16, uint32, uint64, uint
- float32, float64
- complex64, complex128
- byte
- rune
- string

# Array vs slice

Arrays have fixed size and are copied on assignment, while slices are dynamic and reference an underlying array.

# Passing slice to function

A copy of the slice header is passed, but it still points to the same underlying array.

# Slice capacity growth

When capacity is exceeded, Go allocates a new array with larger capacity and copies elements.

# Slice memory leak

A small slice can keep a large underlying array alive, causing memory waste.

# make vs new

make initializes slices, maps, and channels, while new only allocates memory and returns a pointer.

# Check key exists

Use val, ok := m[key] to check existence.

# Nil vs empty slice

Nil slice has no allocation while empty slice has allocation but zero length.

# What is the difference between a string and a byte slice?

A Go string is an immutable sequence of bytes, while a []byte is a mutable slice of bytes.
Strings are commonly used for text, and byte slices are useful when reading files, processing network data or working with JSON encoders.
