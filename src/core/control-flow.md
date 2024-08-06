# Control Flow

## Conditional

Condition is expected to be a bool, other values are not automatically converted to a Boolean.

```rust, editable
fn main() {
    let x = 42;
    if x == 0 {
        println!("zero!");
    } else if x < 100 {
        println!("small");
    } else {
        println!("large");
    }
}
```

We can use `if` as an expression, which returns the value of the last expression in the block (notice missing ;, for returning the value).

We can use if in an expression

```rust, editable
fn main() {
    let x = 10;
    let size = if x < 20 { "small" } else { "large" }; 
    println!("number size: {}", size);
}
```
Note that values in both arm should have same data type since it has to be evaluated and assigned to the `size` variable at runtime.

## Match Case

Match case allows to compare a value against multiple patterns and execute code based on the first matching pattern. Patterns can be made up of 
- literal values, 
- destructured arrays, enums, structs or tuples
- variables
- wildcards
- Placeholders

```rust
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
```


```rust, editable
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),  //
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {state:?}!");
            25
        }
    }
}

fn main() {
    println!("penny: {}", value_in_cents(Coin::Quarter(UsState::Alaska)));
}
```

Match with Option<T>

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}

let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
```

Unlike `if let`, `else if`, `else if let`and `else` , `match` in Rust needs to be exhaustive in the sense that all possibilities for the value in the match expression must be accounted for. One wayy to ensure you've covered every possiblity is to have a catchall pattern for the last arm using a variable name matching any value, or using `_` as a placeholder that doesn't binds to a variable ignoring any other value without any warning.


```rust, editable
    let dice_roll = 9;
    match dice_roll {
        1..=3 => add_fancy_hat(),  // match range of patterns
        5 | 7 => remove_fancy_hat(), // match multiple patterns
        other => move_player(other), // catch all other cases in variable `other`
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn move_player(num_spaces: u8) {}

```

### if let

`if let` is an expression that lets you combine `if` and `let` into a single construct to match one pattern while ignoring the rest. This means less boilerplate code and works the same way without the exhaustive checking of `match`.

```rust

let config_max = Some(3u8);

match config_max {
    Some(max) => println!("The maximum is configured to be {max}"),
    _ => (),  // ignore everything else i.e `None`
}

// alternatively with if let
if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

with else

```rust

let mut count = 0;
match coin {
    Coin::Quarter(state) => println!("State quarter from {state:?}!"),
    _ => count += 1,
}


let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("State quarter from {state:?}!");
} else {
    count += 1;
}
```


## Loops

### While

While loop is a conditional loop that runs till the condition is satisfied.

```rust, editable
fn main() {
    let mut x = 200;
    while x >= 10 {
        x = x / 2;
    }
    println!("Final x: {x}");
}
```

### For

For loop, for-in loop is an iterator loop that iter over an elements of an iterator which can be fixed or could theoretically go on indefinitely.

```rust, editable

fn main() {
    for x in 1..5 {
        println!("x: {x}");
    }

    for x in 1..=5 {
        println!("x: {x}");
    }
    for elem in [1, 2, 3, 4, 5] {
        println!("elem: {elem}");
    }

    for (index, elem) in [1, 2, 3, 4, 5].iter().enumerate() {
        println!("index: {index}, elem: {elem}");
    }

    for i in std::iter::repeat(5) {
    println!("turns out {i} never stops being 5");
    break; // would loop forever otherwise
}
}
```

Under the hood for loops use a concept called "iterators" to handle iterating over different kinds of ranges/collections.


### Loop

Loop is idiomatic infinite loop similar to `while true`.

```rust, editable
fn main() {

}
```

### break and continue

```rust, editable
fn main() {
    let mut i = 0;
    loop {
        i += 1;
        if i > 5 {
            break;
        }
        if i % 2 == 0 {
            continue;
        }
        println!("{}", i);
    }
}
```

Both continue and break can optionally take a label argument which is used to break out of nested loops, can be used for loop, while, for.

```rust, editable
fn main() {
    let s = [[5, 6, 7], [8, 9, 10], [21, 15, 32]];
    let mut elements_searched = 0;
    let target_value = 10;
    'outer: for i in 0..=2 {
        for j in 0..=2 {
            elements_searched += 1;
            if s[i][j] == target_value {
                break 'outer;
            }
        }
    }
    print!("elements searched: {elements_searched}");
}
```

## Example: Fibonacci Number

```rust, editable

fn fib(n: u32) -> u32 {
    if n < 2 {
        todo!("Implement this");
    } else {
        todo!("Implement this");
    }
}

fn main() {
    let n = 20;
    println!("fib({n}) = {}", fib(n));
}
```

<details>
<summary>Solution</summary>

```rust, editable

fn fib(n: u32) -> u32 {
    if n < 2 {
        return n;
    } else {
        return fib(n - 1) + fib(n - 2);
    }
}

fn main() {
    let n = 20;
    println!("fib({n}) = {}", fib(n));
}
```
</details>