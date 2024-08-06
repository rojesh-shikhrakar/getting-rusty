# Functions

The last expression in a function body (or any block) becomes the return value if `;` is ommited at the end of the expression. The return keyword can also be used for early return. Some functions have no return value, and return the 'unit type', (). The compiler will infer this if the -> () return type is omitted.

```rust, editable
fn gcd(a: u32, b: u32) -> u32 { // declaring fn with parameters annotated by their type -> return type
    if b > 0 {
        gcd(b, a % b)   // last expression in the block is a returned if ; is ommited
    } else {
        a               // last expression in the block is a returned if ; is ommited 
    }
}

fn main() {
    println!("gcd: {}", gcd(143, 52));  // calling gcd with argument values
}
```

Rust code uses snake case as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words.

Overloading is not supported -- each function has a single implementation.
Always takes a fixed number of parameters. Default arguments are not supported. Macros can be used to support variadic functions.
Always takes a single set of parameter types. These types can be generic.


Statements are instructions that perform some action and do not return a value. Expressions evaluate to a resultant value. Rust is an expression-based language.