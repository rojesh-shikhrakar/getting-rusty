# Structs and Enums

Structure (struct) is a custom data type that lets you package together multiple related, named values (fields) in a meaniful group similar to object's data attributes.  We create an instance of struct by specifying concrete values for each fields in key:value pairs, and access the values using the dot notation. The individual fields cannot be marked as mutable hence the entire instance needs to be declared as mutable.


```rust, editable

struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main(){
    let mut user1 = User {
        active: true, 
        sign_in_count: 1,
        username: String::from("someone123"), 
        email: String::from("someone@example.com"), 
        };
    
    user1.sign_in_count += 1;

    println!("user1.username: {}", user1.username);
    
    let mut user2 = User {
        email: String::from("someone456@example.com"),
        ..user1   // remaining fields of user1 is copied into user2
        // string is also copied, not a stack only copy hence, user1 won't be available
    };
    
    println!("user2.sign_in_count: {}", user2.sign_in_count);
    // println!("user1.username: {}", user1.username); 

}

// functions can use the field init shorthand syntax rather than repeat each field names
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

## Special Structs

Tuple Structs are similar to Tuples, without field names

```rust, editable
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

unit-like structurs don't have any fields at all, similar to `()` the unit type. These are useful when you need to implement a trait on some type but don’t have any data that you want to store in the type itself.

```rust, editable
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

## Enum

Enumeration (enum) allows to define a type by enumerating its possible variants. Enum give you a way of saying a value is one of a possible set of values, not both at the same time. We can even optionally put data directly into each enum variant as well by defining its associated data type.

```rust
enum IpAddr {
        V4(String),
        V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));

fn route(ip_kind: IpAddrKind) {}  // to call function with either variant
```

The standard library define `IpAddr` enum with struct as data type instead of string.

A popular enum defined by standard library is `Option` included in the prelude, which encodes the  common scenario in which a value could be something or it could be nothing (instead of null feature). With this functionality compiler can check whether you have handled all the cases and prevent bugs related to null references. You have to convert an `Option<T>` to a `T` before you can perform T operations with it. This helps catch one of the most common issues with null: assuming that something isn’t null when it actually is.

```rust
enum Option<T> {
    None,
    Some(T),
}

let absent_number: Option<i32> = None;
let some_number = Some(5);

```

Another most common enum is [Result](https://doc.rust-lang.org/std/result/enum.Result.html) that represent either success `Ok` or failure `Err`.
## Trait

## Methods

All functions defined within an impl block are called associated functions.

Methods are associated function that specifies the behaivors associated with a struct type. Unlike functions they are defined wihtin the context of a struct (or enum or a trait object). Their first parameter is always a reference to the struct instance (self), which represent instance of the struct the method is being called on. We don't have to repeat the type of self in every method. Also all the things we can do with an instance of a type is placed in one `impl` block rather than in various places in the library. However, each struct is allowed to have multiple impl blocks.

```rust, editable

struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 { // &self is short for self: &Self ; alias for the type
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("The area of the rectangle is {}", rect1.area());
}
```

Unlike C/C++ which uses `.` for calling method on object and `->` for calling method on pointer, Rust has automatic referencing and dereferencing. When you call a method, Rust automatically adds in `&`, `&mut`, or `*` so object matches the signature of the method.
`p1.distance(&p2);` is equivalent to  `(&p1).distance(&p2);` Given the receiver and name of a method, Rust can figure out definitively whether the method is reading (`&self`), mutating (`&mut self`), or consuming (`self`). 


We can define associated functions that don’t have self as their first parameter (and thus are not methods) eg: is `String::from` function that’s defined on the String type. These associated functions are often used for constructors that will return a new instance of the struct. These are often called new, but new isn’t a special name and isn’t built into the language.

```rust, editable
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

To call this associated function, we use the `::` syntax with the struct name; `let sq = Rectangle::square(3);` is an example. This function is namespaced by the struct.

## Examples

```rust, editable

#[derive(Debug)]
struct Fibonacci {
    current: u8,
    previous: u8,
}

impl Fibonacci {
    fn new() -> Self {
        Self {
            current: 0,
            previous: 0,
        }
    }
}

impl Iterator for Fibonacci {
    type Item = u8;

    fn next(&mut self) -> Option<Self::Item> {
        if self.current == 0 {
            self.current = 1;
            return Some(self.current);
        }

        let next_value = self.previous.checked_add(self.current)?;
        self.previous = self.current;
        self.current = next_value;
        Some(self.current)
    }
}

fn main() {
    for fb in Fibonacci::new() {
        println!("{fb}");
        if fb > 30 {
            break;
        }
    }
    println!("------");
    for fb in Fibonacci::new() {
        println!("{fb}");
    }
}

```