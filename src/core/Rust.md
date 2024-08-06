# Getting Rusty

The goal of this [MD book](https://rust-lang.github.io/mdBook/index.html) is to track my journey of getting Rusty and cover the full spectrum of Rust. The standard process of learning rust is throught the [official Rust Programming Book](https://doc.rust-lang.org/book/), followed by interactive [Rustling Course](https://github.com/rust-lang/rustlings/). For those with existing background working with a static compiler, one can also start with [Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/) or the [Comprehensive Rust - Google](https://google.github.io/comprehensive-rust/). Many examples in this are adopted from these resources.

Rust is a multi-paradigm systems programming language focusing on safety, speed, and concurrency.

<details>
<summary>Multi-paradigm</summary>

- Imperative Programming
- Object Oriented Programming with struct, enum, traits, methods
- Functional Programming: immutability, higher-order functions, pattern matching

</details>

<details>
<summary>Compile time memory safety</summary>

Different type of memory bugs are prevented at compile time.

- No uninitialized variables.
- No double-frees.
- No use-after-free.
- No NULL pointers: no issue of dereferencing null pointers, dangling pointers.
- No forgotten locked mutexes.
- No data races between threads.
- No iterator invalidation.
- No undefined runtime behavior - what a Rust statement does is never left unspecified
- Array access is bounds checked.
- Integer overflow is defined (panic or wrap-around).
- Many abstractions such as iterators are zero-cost. There is no garbage collector, so you can use exactly as much memory requried at the given time.

Furthermore, good compiler error messages allows writing and debugging rust code easy and more productive.

</details>

<details>
<summary>Rust is fast and resource efficient</summary>

- Rust is statically compiled with `rustc` which uses LLVM as its backend. Performance (runtime and memory) is comparable to C/C++. 
- Full support for concurrency using OS threads with mutexes and channels. Refered as "Fearless concurrency" increases reliability on the compiler to ensure correctness at runtime. 
- also provides unsafe use of rust for even faster operations [Nomicon](https://doc.rust-lang.org/nomicon/)

</details>

<details>
<summary>with expressive language features</summary>

- Generics.
- No overhead foreign function interface (FFI). Fucntion call be rust and C have identical performance to C function calls.
- Built-in dependency manager: cargo.
- Built-in support for testing.
- Excellent Language Server Protocol support.
</details>

<details>
<summary> Other features </summary>

- Strong, static yet expressive type system influenced by Haskell. Types allows to check potential problems and avoid them.
- Concurrency can be done with any technique with thread saftey through the same type system ensuring memory saftey
- Cross platform: compile to different systems, embedded systems and even web as WebAssembly (WASM)
- C interoperability, but use of C reduces memory safety
- supports many [platforms and architectures](https://doc.rust-lang.org/nightly/rustc/platform-support.html): x86, ARM, WASM, Linux, Mac, Windows, ...
</details>

Rust has been voted the "most loved programming language" in the Stack Overflow Developer survey since 2016. But it is not the only language that has been voted as the most loved programming language. [Google gathered data from their engineers to understand the rust learning curve, and found that they were proficient in Rust in less than 2 months](https://opensource.googleblog.com/2023/06/rust-fact-vs-fiction-5-insights-from-googles-rust-journey-2022.html)


## How to use this Guide

There are several useful keyboard shortcuts in mdBook:

- `Arrow-Left`: Navigate to the previous page.
- `Arrow-Right`: Navigate to the next page.
- `Ctrl + Enter`: Execute the code sample that has focus.
- `s`: Activate the search bar.

There are code blocks like the following in this book which allows you to edit and run the code as you wish.

```rust, editable
fn main() {
    println!("Hello 🌍!");
}
```

And yes it looks very like C/C++, `main` function define with `fn` keytword is the entry point of the program. Blocks are delimited by curly braces and needs a semi-colon to end a statement.
