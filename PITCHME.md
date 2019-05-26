# ![Rust](assets/img/rust.png) for Rubyists

---

## Rust in Baden

- Population of ~4100
- Elevation of 169 m

![](assets/img/rust_church.jpg)

---

### How I came to rust

- Looking for a *20% Language*
- Looking for "modern C"

---

### Selling points of rust

- Performance
- Memory-Safety without a GC
- Zero-Cost-Abstrations
- Great little crab mascot
![](assets/img/rustacean.png)

---

### Designed to replace C/C++

---

### What it's not

- Easy to pick up
- (unless you are well versed in C++ **and** Haskell)

---?gist=https://gist.github.com/Haniyya/36e90e3d630de57151b0b25ad17f9d79
#### Basic hello world
@[1-1](Entry function signature. Returns nothing.)
@[2-2](Macro to print "Hello, world!")
@[3-3](End of function)
---

### Differences to **ruby**

@ul
- The stack is back
- Immutable by default
- No "real" meta-programming
- Data/Behaviour separated
- **Ownership**
@ulend

---

### Ownership (fighting the Borrow-Checker)

1. Every value has **one**(1) variable that "owns" it
1. If the variable goes out of scope, the value it freed

```rust
  let x = "cake";
  eat(x);
  have_it_too(x); // Won't compile
```
1. You can **borrow** though
```rust
  let x = "cake";
  eat(&x); // Only borrowed. So we give the cake back after eating it. Urgh...
  have_it_too(x); // Will compile!
```
---

#### How does that help?

@ul
- No garbage collector needed!
- No more memory allocation!
- Fearless concurrency!
    - No more data races
@ulend

---

#### How does that hinder you?

- Global mutable state is possible, but really discouraged. So don't.
- Stuff like a graph is non trivial:

```rust
struct Node<T> {
  value: T,
  neighbors: Vec<Node<T>>,
}

let first_node = Node { value: 7, neighbors: vec![] };
// first_node is **moved** here
let second_node = Node { value: 5, neighbors: vec![first_node] };
// Use after move. Won't compile.
let third_node = Node { value: 20, neighbors: vec![first_node] };
```
---
### Immutability by default
- All variables are immutable by default
```rust
let x = 7;
x = 9; // NoNoNo!
```
- Possible to make things mutable
```rust
let mut x = 7;
x = 9; // Ok!
```
---?gist=https://gist.github.com/Haniyya/b2ff3f82b1519f67d2c7a72b102801b7
### Expressive Signatures
@[1-2](Consume the value.)
@[4-5](Consume the value and return a **new** value.)
@[7-8](Read the value. Return the ownership afterwards.)
@[10-11](Change the value and return it.)
---
### Zero cost abstractions
  - High level concepts in low level language
  - Most gets compiled away

```rust
let numbers = vec![1, 2, 3, 4];
numbers.iter().map(|i| i + 2).map(|i| i * 2) // Lazy and chainable
```
---
### Monads! (I think)

```rust

fn sqrt(a: f32) -> Option<f32> {
    match a {
        _ if a < 0.0 => None,
        _ => Some(a.sqrt()),
    }
}

fn main() {
    println!("{:?}", sqrt(9.0).map(|n| n + 2.0));  // Print Some(5)
    println!("{:?}", sqrt(-9.0).map(|n| n + 2.0)); // Print None
}
```
---
### Error handling
```rust
use std::num::ParseIntError;

// As with `Option`, we can use combinators such as `map()`.
// This function is otherwise identical to the one above and reads:
// Modify n if the value is valid, otherwise pass on the error.
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    let twenty = multiply("10", "2");
    print(twenty);

    let tt = multiply("t", "2");
    print(tt);
}

// Output:
// n is 20
// Error: invalid digit found in string
```
---?gist=https://gist.github.com/Haniyya/c59eae6b369ac152914999704f84ac6b
### Fearless concurrency
@[5-5](Wrap a mutex in an Atomic reference counter)
@[9-9](Get a new reference to the mutex. Increase the counter)
@[15-16](Arc gets dropped, decreases reference count by 1)
@[18-20](All Arcs dropped, deallocate counter)
@[22-22](Print "Result: 10")
---
### Rubyists in rust
1. Yehuda Katz went from bundler to cargo
1. Sean Griffin went from active-record to diesel
---
### Some cool libraries
1. Rayon
1. Serde
1. Bindgen
1. Helix
1. Yew
---
### Who uses it?
1. Firefox
1. Dropbox
1. Cloudflare
1. Parity (ethereum client)
1. Facebook (soon)
1. You (soon)
---
### Wanna try it?
![Rust lang website](assets/img/frame.png)
