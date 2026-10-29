# What is an interface in Go?

- An interface is a collection of method signatures that defines behavior. 
- It allows different types to be used interchangeably if they implement the same methods.
OR
- An interface defines what a type can do, not how it does it.

# Why use interfaces?

- Interfaces allow us to define behavior instead of depending on a specific concrete implementation. This helps us to achieve loose coupling (components have weak interdependencies) and polymorphism. 
- Different types can satisfy the same interface, so we can easily replace implementations, which is also useful for testing and dependency injection.

Dependency injection is a programming technique where an object receives the other things it needs (called dependencies) from the outside, rather than creating or managing them itself.

# How is an interface implemented in Go?

- Go uses implicit interface implementation. A type satisfies an interface when it implements all the methods required by that interface. We don't explicitly declare that the type implements the interface.

# What is an empty interface?

- An empty interface interface{} has no methods, so it can hold values of any type. It is commonly used when the type is not known at compile time.

# Type assertion vs type switch?

- Type assertion checks and extracts a specific concrete type from an interface, while type switch is used to handle multiple possible concrete types safely.

# What is the difference between an interface and a struct?
# What is a nil interface?
# Can an interface hold a pointer?
# What is type assertion?
# What is a type switch?
# How does interface composition work?

# What is the difference between an interface and a concrete type?

A concrete type defines actual data or behavior, such as a struct. An interface defines the behavior that another type must provide.
For example, MySQLUserRepository can be a concrete type, while UserRepository can be an interface. The service depends on the interface instead of directly depending on the MySQL implementation.
This reduces coupling and makes it easier to test or change implementations.

# Does Go have an implements keyword?

- No. Go uses implicit interface implementation, so there is no implements keyword.

# Embedding interface?

- Interface embedding allows one interface to include another interface's method requirements. The resulting interface requires all methods from the embedded interfaces plus any additional methods it declares.

# What are generics in Go?

Generics let us write functions and data structures that work with different types without duplicating the same logic for each type.

For example, I can write a generic function that returns the first element of a slice, regardless of whether it contains integers or strings.

func First[T any](items []T) (T, bool) {
    var zero T

    if len(items) == 0 {
        return zero, false
    }

    return items[0], true
}


Here, T is a type parameter, and any means that the type can be any Go type. Generics are useful for reusable utilities, but I would not use them when a normal function or interface is simpler.