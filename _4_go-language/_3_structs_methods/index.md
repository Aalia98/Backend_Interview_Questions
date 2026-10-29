## ------------------------------ Struct -----------------------------------

# What is a struct?

- A struct is a user-defined data type in Go that groups related fields together.
- It is used to model real-world entities like User, Order, or Product.

# Why do we use structs?

- Structs allow us to group related data of different types into a single custom type.

# Struct vs class?

- Go doesn't have traditional classes. It uses structs to represent data and methods attached to types to define behavior.

# How do you initialize a struct?

user := User {
    ID: 1,
    Name: "Aalia",
    Email: "aalia@example.com",
  }

# What happens if I don't initialize struct fields?

- Struct fields also receive their respective zero values.

# JSON tags in struct?

- JSON tags control field names, omit empty fields, or skip fields.
- Example: json:"name,omitempty".

# Embedding struct ?

- Struct embedding includes one struct inside another without field names, making embedded fields directly accessible.
- Example:
  myCar := Car{
  Engine: Engine{
  Horsepower: 300,
  FuelType: "Premium Electric",
  },
  Model: "Model S",
  Brand: "Tesla",
  }

# goto statement in go?

- The goto statement in Go provides an unconditional jump from the goto line to a labeled statement within the very same function.
- Example :
  package main

  import "fmt"

  func main() {
  fmt.Println("Start")

        goto SkipAhead // Jumps directly to the label

        fmt.Println("This will never print")

  SkipAhead:
  fmt.Println("End")
  }

## ------------------------------ Methods -----------------------------------

# What's the difference between a function and a method?

- A function is independent, while a method is a function associated with a type through a receiver.

# What is a method receiver?

- A receiver is the value that a method operates on.
- It allows us to define behavior for structs.

# Difference between value receiver and pointer receiver?

- A value receiver works on a copy of the struct, while a pointer receiver works on the original struct.
- Pointer receivers are preferred when we want to modify data or avoid copying large structs.

# What is a variadic function?

Functions in general accept only a fixed number of arguments. A variadic function is a function that accepts a variable number of arguments. If the last parameter of a function definition is prefixed by ellipsis (…), then the function can accept any number of arguments for the last parameter.

# Anonymous Functions?

- Functions declared without names, called anonymous function also called function literals or lambdas.

# Closure ?

- Closure is an anonymous function value that references variables from outside its body.
- The function is "bound" to these variables, meaning it can access, update, and remember them between executions, even after the surrounding outer function has finished running.

# Named Return Values?

- Named return values allow you to explicitly define variable names for your return parameters right in the function signature. They are treated as regular variables defined at the top of the function and are initialized to their respective zero values.

# Call By Value?

- Go is strictly a call-by-value language. This means that whenever you pass an argument to a function, Go always creates a copy of the data and passes that copy to the function. Any modifications made to the parameter inside the function will not affect the original variable outside of it.