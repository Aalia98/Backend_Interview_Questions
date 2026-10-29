# Q1. What is a class?

- A class is a blueprint that defines the data and behavior of objects.

# Q2. What is an object?

- Object is an instance created from blueprint.

# Q3. In this Go code:

    type User struct {
        ID    int
        Name  string
        Email string
    }

    user := User{
        ID:    1,
        Name:  "Aalia",
        Email: "aalia@example.com",
    }
# What is: User? and user?

- User is a struct type.
- user is a value/instance of the User struct type.

# Q4. Does Go have classes like Java? Why/why not?

- No, Go doesn't have traditional classes like Java. Go uses struct to define data and methods. Go achieves OOP-like behavior using: Structs, Methods, Interfaces, Composition / Embedding