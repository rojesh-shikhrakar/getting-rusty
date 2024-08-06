# Variables and Data Types

A variable is a named abstraction of the memory (storage location) which holds a data. It has a name, a datatype, value, reference to memory location. Rust provides type safety via static typing, i.e. type of the variables must be known at compile time. Hence, the type must be declared or infered based on the value assigned during the compilation type checking. Static typing helps catch type errors early in the development process, reducing the likelihood of runtime errors related to type mismatches. 

 Variable bindings are made with let, and the type is declared with a colon. 

```rust, editable

fn main() {
    let x: i32 = 42;  
    let y = 17;  // type i32 inferred from value during compilation 
    println!("x: {x}");
    // x = 24;    // cannot assign twice to immutable vairable
    // println!("x: {x}");
}

```

Variables are immutable by default. We can make it mutable using `mut` keyword. We can only change its value not the type.

```rust, editable

fn main() {
    let mut x: i32 = 42;
    println!("x: {x}");
    x = 24;
    println!("x: {x}");
}
```

This doesn't mean let creates constants, constants are values that are bound to a name, must be type annotated, and are not allowed `mut` usage to change it. Constants can be declared in any scope, usually defined in global scope.

```rust, editable
const PI: f32 = 3.14159265;

fn main(){
    let r: f32 = 2.0;
    println!("{}",PI*r.powf(2.0));
}

```

Even if the variables cannot be changed, one can reuse the variable name by redeclaring the variable. The first variable is said to be shadowed by the second. Furthermore we can even create a new type.

```rust, editable

fn main() {
    let x = "Five"; 
    let x = x.len();  // shadowing to store different data type.

    {
        let x = x * 2;  // shadowing to create a new variable in the inner scope
        println!("x in the inner scope is: {x}");
    }

    println!("x in outer scope is: {x}");
}
```

## Data Types

```rust, ignore
use std::io;

fn main(){
    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)              // read line from stdin
        .expect("Failed to read line");     // error handling

    let guess: i32 = guess.trim().parse().expect("Not a number!");  // trim end spaces and parse to annotated format

    println!("{}",guess);    
}
```

Basic built-in types, and the syntax for literal values of each type.

|                        | Types                                      | Literals                       |
| ---------------------- | ------------------------------------------ | ------------------------------ |
| Signed integers        | `i8`, `i16`, `i32`, `i64`, `i128`, `isize` | `-10`, `0`, `1_000`, `123_i64` |
| Unsigned integers      | `u8`, `u16`, `u32`, `u64`, `u128`, `usize` | `0`, `123`, `10_u16`           |
| Floating point numbers | `f32`, `f64`                               | `3.14`, `-10.0e20`, `2_f32`    |
| Unicode scalar values  | `char`                                     | `'a'`, `'α'`, `'∞'` , `'😍'`   |
| Booleans               | `bool`                                     | `true`, `false`                |

The types have widths as follows:

- `iN`, `uN`, and `fN` are _N_ bits wide,
- `isize` and `usize` are the width of a pointer depending on the computer architecture,
- `char` is 32 bits wide,
- `bool` is 8 bits wide.

The signed numbers are stored using two's complement.

Integer overflow can occur when we change a variable to outside of the range. eg: 256  to u8 with range 0 - 255
- in debug mode, Rust includes checks for integer overflow that cause your program to panic at runtime if this behavior occurs.
- in release mode i.e compiled with `--release` flag, Rust doesn't panic but performs two’s complement wrapping, i.e 256 becomes 0, 257 becomes 1,...

Relying on integer overflow’s wrapping behavior is considered an error, hence rust provides families of methods to handle them explicitly. 
- Wrap in all modes with the wrapping_* methods, such as wrapping_add.
- Return the None value if there is overflow with the checked_* methods.
- Return the value and a boolean indicating whether there was overflow with the overflowing_* methods.
- Saturate at the value’s minimum or maximum values with the saturating_* methods.  e.g., (a * b).saturating_add(b * c).saturating_add(c * a).

Rust provides addition, subtraction, multiplication, division, and remainder operators. Here's list of [Operators](https://doc.rust-lang.org/book/appendix-02-operators.html)

Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

Array is a datatype that stores a 'N' number of values of the same type 'T'. Here length 'N' (a compile-time constant) is part of the type [T, N] hence [u8; 3] and [u8; 4] are considered two different types. 

```rust, editable
fn main() {
    let mut a: [i8; 10] = [42; 10];
    a[5] = 0;   // indexing to access array
    println!("a: {a:?}");
}
```

Try accessing an out-of-bounds array element. Array accesses are checked at runtime and panic when index is beyond the arrays bound. Rust can usually optimize these checks away, and they can be avoided using unsafe Rust.

We can use literals to assign values to arrays.

The println! macro asks for the debug implementation with the ? format parameter: {} gives the default output, {:?} gives the debug output. Types such as integers and strings implement the default output, but arrays only implement the debug output. This means that we must use debug output here.

Adding #, eg {a:#?}, invokes a "pretty printing" format, which can be easier to read.

Like arrays, tuples have a fixed length but tuples can group together values of different types into a compound type. Fields of a tuple can be accessed by the period and the index of the value, e.g. t.0, t.1. The empty tuple () is referred to as the "unit type" and signifies Void or absence of a return value.

```rust, editable
fn main() {
    let t: (i8, bool) = (7, true);
    println!("t.0: {}", t.0);  //indexing to access tuple
    println!("t.1: {}", t.1);
    let (x, y) = t;  // destructuring tuple to two variables x, y
    println!("{x}");
}
```

## Type inference

Rust compiler infers types based on constraints given by variable declarations and usages. Machine code generated by such declaration is identical to the explicit declaration of a type.

```rust, editable
fn takes_u32(x: u32) {
    println!("u32: {x}");
}

fn takes_i8(y: i8) {
    println!("i8: {y}");
}

fn main() {
    let x = 10;
    let y = 20;

    takes_u32(x);
    takes_i8(y);
    // takes_u32(y);
}
```

## Type Conversion



## Blocks and Scopes

A block in Rust contains a sequence of expressions, enclosed by braces {}. Each block has a value and a type, which are those of the last expression of the block:

```rust, editable
fn main() {
    let z = 13;
    let x = {
        let y = 10;
        println!("y: {y}");
        z - y  // if commented block returns ()
    };
    println!("x: {x}");
}
```

A variable's scope is limited to the enclosing block.

You can shadow variables, both those from outer scopes and variables from the same scope:

```rust, editable
fn main() {
    let a = 10;
    println!("before: {a}");
    {
        let a = "Hi";
        println!("inner scope: {a}");

        let a = true;
        println!("shadowed in inner scope: {a}");
    }

    println!("after: {a}");
}
```
Shadowing is different from mutation, because after shadowing both variable's memory locations exist at the same time. Both are available under the same name, depending where you use it in the code.

## Memory Management & Ownership

All programs need to manage memory for efficient memory utilization, prevent memory leaks for safety and program stability. There are few approaches for memory management:
1. Manual memory allocation and deallocation (eg in C/C++) for fine-grained control which can lead to memory leaks and segmentation faults
2. Automatic memory management using garbage collection (GC) (eg: Java, Python) regularly check for unused memory and free the memory. Some uses Automatic Reference Counting (ARC) (eg: swift) to keep count of object in memory and deallocates the memory if count drops to zero.
    - If we forget to clean, it'll waste memory, 
    - if we do it too early it would create invalid variable, 
    - if we do it twice, it can create double-free bug.
3. Ownership and borrowing (rust): set of rules enforces at compile time, where each piece of data or value has a single owner at a time and memory is automatically freed (i.e. the value is dropped) when the owner goes out of scope. 

Stack and Heap Allocation
| |Stack | Heap |
|---|---|---|
|Structure| Contiguous block of memory, hence size known and fixed| non-contiguous allocation, less organized, memory allocator finds empty spot and returns a pointer to use it|
|Usage | static memory: local variable, function call management | dynamic memory: objects, data structure|
| Management | By compiler allocated when function called, deallocated when exits | mannually allocated, deallocated depending on programming language |
| speed| faster alloc/dealloc & access | slower alloc/dealloc & access due to complexity, following pointers|
| scope | limited to function/block| usually global, as long as reference exists|
| safety| follows LIFO to reduce memory corruption| prone to memory leaks and fragmentation|

When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap) and the function’s local variables get pushed onto the stack. When the function is over, those values get popped off the stack. The main purpose of ownership is to manage heap data.

Most of the data primitive data types have known, fixed size hence are stored on stack and easily popped off when their scope is over. String literal eg: "hello" are fixed and known at compile time. Dynamic data type such as `String`, whose size are unknown at compile time and whose size might change during runtime need to allocate memory on heap. 

```rust, editable
{
let mut s = String::from("hello"); // Memory requested from memory allocator at runtime

s.push_str(", world!"); // push_str() appends a literal to a String

println!("{s}");
} // scope of s is over and s is no longer valid
```
 
In Rust the memory is automatically returned once the variable that owns it goes out of scope (without need of GC). Rust calls a function called drop automatically at the closing curly bracket. This is similar to Resource Acquisition Is Initialization (RAII) pattern in C++.


```rust, editable
let x = 5;  // data in stack
let y = x;  // copy of value of x is binded to y

let s1 = String::from("hello");  //String is made up of ptr to heap, len, capacity
let s2 = s1;  // String data (ptr, len, capacity) is copied not the actual string from the heap, same data is being used

// println!({"s1"})  // s1 is invalid after move
```

since both s1, s2 points to same memory, both might try to free the memory "double-free error". To ensure memory safety, after `let s2 = s1;` Rust consider `s1` to be moved to `s2` and `s1` is no longer valid. 

This type of copy is called a shallow copy in other language, but because of invalidation of first variable, it is known as move in rust.By design choice, Rust will never automatically create "deep" copies of data.

```rust, editable
let s1 = String::from("hello");
let s2 = s1.clone();  // create deep copy of heap data

println!("s1 = {s1}, s2 = {s2}");
```
Rust has `Copy` trait that we can place on types that are stored on stack such as integers, if a type implements the `Copy` trait, variables that use it do not move, but rather are trivially copied, making them still valid after assignment to another variable. `Copy` is not allowed if `Drop` is implemented.

Passing a value to a function is similar to assigning a value to a variable.

```rust, editable

fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
    
    // s is no longer valid here

    let x = 5;     // x comes into scope

    makes_copy(x);   // x would move into the function,
    
    // but i32 is Copy, so it's still valid & can be used

} // Here, x goes out of scope, then s. But because s's value was moved, nothing special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} //some_string goes out of scope and `drop` is called. The backing memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope.

```

```rust, editable

fn main() {
    let s1 = gives_ownership();         //  moves return value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into function  which moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {      // move its return value into the function  that calls it

    let some_string = String::from("yours"); // comes into scope

    some_string    // returned and moves out to the calling  function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```
When a variable that includes data on the heap goes out of scope, the value will be cleaned up by drop unless ownership of the data has been moved to another variable.

Taking ownership and then returning ownership with every function is a bit tedious, if we want to let a function use a value without transferring owernship through reference .

## References & Borrowing

Reference is like a pointer to the address of the data storage, whose data is owned by some other variable. Unlike a pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference. The action of creating a reference is called borrowing. The borrowed references are immutable by default. Multiple **immutable references** can exist simultaneously. We can also create a **mutable reference**, but *only one mutable reference can exist at a time to prevent data races*. Data race condition occur when two or more pointer access the same data at the same time and at least one is being used to write to the data without any mechanism to synchronize access to the data. For the same reason, we also cannot have a mutable reference while we have an immutable one to the same value, only allowed if the scope of immutable reference ends before creating mutable reference.

```rust, editable

fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);  // creates and passes a reference

    println!("The length of '{s1}' is {len}.");

    let s2 = &s1 ;  //s1 is unused after so mutable reference is fine

    change(&mut s1);  // pass mutable reference

    println!("{s}");
}

fn calculate_length(s: &String) -> usize {  // takes in a reference
    s.len()
}

fn change (s: &mut String){
    s.push_str(", world");
}
```

we can use curly brackets to create a new scope, allowing for multiple mutable references, just not simultaneous ones:

```rust, editable
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;

```

For a contiguous sequence of elements in a collection, it might be useful to pass reference of a slice of the elements rather than the whole collection. 

```rust, editable
fn first_word(s: &String) -> usize {  // returning usize
    let bytes = s.as_bytes();  // convert strings to array of bytes

    for (i, &item) in bytes.iter().enumerate() {  // iterate over byte as indexed tuple
        if item == b' ' {  //  search for the byte representing space
            return i;
        }
    }

    s.len()  // meaningful and valid only as long as string is valid
}

fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s); // word will get the value 5

    s.clear(); // this empties the String, making it equal to ""

    // word still has the value 5 here, but there's no more string that
    // we could meaningfully use the value 5 with. word is now totally invalid!
}
```


```rust, editable
    let s = String::from("hello world");

    let hello = &s[0..5];  // first and last index, but stores start and length of slice, 
    let world = &s[6..11];
    let word = &s[..]; // both first and last index can be dropped to take entire string

```

```rust, editable
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}

fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // compiler indicates error!

    println!("the first word is: {word}");
}
```

The compiler will ensure the references into the String remain valid. Rust disallows the mutable reference in clear and the immutable reference in word from existing at the same time, and compilation fails. Not only has Rust made our API easier to use, but it has also eliminated an entire class of errors at compile time!

The type of string literal is &str: a slice pointing to  specific point of the binary. This is also why string literals are immutable; &str is an immutable reference.

```rust
fn first_word(s: &str) -> &str {

```


dangling pointer — a pointer that references a location in memory that may have been given to someone else—by freeing some memory while preserving a pointer to that memory. In Rust, by contrast, the compiler guarantees that references will never be dangling references: if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.

```rust, editable
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger!
```
The solution is to return the string directly.



TODO Deferencing with * operator

## Lifetimes

lifetimes is used to track how long references are valid. This ensures that references do not outlive the data they point to, preventing dangling references.