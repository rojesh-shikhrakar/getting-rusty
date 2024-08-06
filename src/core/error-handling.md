# Error Handling

Rust requires you to acknowledge the possibility of an error and take some action before your code will compile. Rust groups errors into two major categories: 
1. recoverable: eg *file not found* error -> report and retry without stoping the program
    - use `Result<T,E>` enum with two variants `Ok(T)` and `Err(E)`
2. unrecoverable errors: eg: Runtime failures like failed bounds checks, out of memory, or file I/O errors
    - use `panic!` to stop execution
    - use Non-panicking APIs (eg: `Vec::get`) if crashing is not acceptable.

## Unrecoverable error with `panic!`

Rust handles fatal errors (unrecoverable and unexpected) with a "panic". panic can be triggered
- by taking an action that causes thhe code to panic, usually as symptoms of bugs in program logic.
- explicitly calling `panic!` macro

By default, these panics will print a failure message, unwind, clean up the stack, and quit. Unwinding means rust walk back up the stack and cleans up the data from each function it encounters. The unwinding can be caught. We can also abort immediately on panic without cleaning up , which makes the binary as small as possible by adding the following line in `Cargo.toml`.

```TOML
[profile.release]
panic = 'abort'
```

```rust, editable
fn main() {
    panic!("crash and burn");
}
```

Error Message : src/main.rs:2:5 indicates that it’s the second line, fifth character of our src/main.rs file. 

In many cases call to panic! might be part of some other function calls in different files, in such cases the filename and line number of panic! might be reported, not the line of code that eventually led to panic! call.

```rust, editable
fn main() {
    let v = vec![1, 2, 3];

    v[99];  // accessing invalid index 
}
```

In other languages, such *Buffer overread* leads to security vulnerabilities, but Rust will trigger a panic and stop execution.
Rust provides a note telling to run with `RUST_BACKTRACE` to get a backtrace (list of all function call leading to panic) of exactly what happened to cause the error.

`$ RUST_BACKTRACE=1 cargo run`

Catching the panic

```rust, editable

use std::panic;

fn main() {
    let result = panic::catch_unwind(|| "No problem here!");
    println!("{result:?}");

    let result = panic::catch_unwind(|| {
        panic!("oh no!");
    });
    println!("{result:?}");
}
```

## Recoverable error with `Result<T,E>`

Many functions in rust returns a Result to state if it succeeded or failed in its operation. While reading a file, the file might not exist, or we might not have permission to access the file, leading to different types of errors, Result enum can convey such information.

```rust, editable

use std::fs::File;       // T for file open
use std::io::ErrorKind;  // E variants

fn main() {
    let greeting_file_result = File::open("hello.txt");  // returns a result

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() { 
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {e:?}"),
            },
            other_error => {
                panic!("Problem opening the file: {other_error:?}");
            }
        },
    };
}
```

Match can be verbose, and might not communicate intent well. Helper methods defined on Result can be helpful for more specific task. Such as - unwrap(): return value inside Ok or panic! if Err. 
- expect(Msg): allows to return good message    
- unwrap_or_else.

```rust, editable
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {error:?}");
            })
        } else {
            panic!("Problem opening the file: {error:?}");
        }
    });
}
```

