# ![Rust](assets/img/rust.png) for Rubyists

---

## Rust in Baden

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
![](assets/img/rustacean.png)

---

### Designed to replace C/C++

---

### What it's not

- Easy to pick up
- (unless you are well versed in C++ **and** Haskell)

---?gist=https://gist.github.com/Haniyya/36e90e3d630de57151b0b25ad17f9d79
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

- Global mutable state is possible, but really hard to do. So don't.
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
@[1-2](Consume the value.)
@[4-5](Consume the value and return a **new** value.)
@[7-8](Read the value. Return the ownership afterwards.)
@[10-11](Change the value and return it.)
---
@ul
- Zero cost abstractions
  - High level concepts in low level language
  - Most gets compiled away

```rust
let numbers = vec![1, 2, 3, 4];
numbers.iter().map(|i| i + 2).map(|i| i * 2) // Lazy and chainable
```
- Fearless concurrency
  - Very easy to parallelize operations
  - Compiler guarantees no-data-races
@ulend

```rust
let numbers = vec![1, 2, 3, 4];
// par_iter parallelizes execution of the iterator based on a thread-pool
numbers.par_iter().map(|i| i + 2).map(|i| i * 2) // Lazy and chainable
```
1. Firefox was thus able to parallelize CSS-rendering
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
---
### Who uses it?
1. Firefox
1. Dropbox
1. Cloudflare
1. Parity (ethereum client)
1. Facebook (soon)
1. You (soon)
---
