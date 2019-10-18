# Introduction
- `cargo new`
- Cargo.toml
- `let` (immutable)
- `let mut` (mutable)
- "associated function"
- `&` to reference
- references immutable by default
- enums have set of possible variants, e.g. `Ok`, `Err`
- Result types encode error info
- `[dependencies]`
- `cargo update`
- `Cargo.lock`
- trails define methods
- `cargo doc --open`
- `match` expressions have arms
- arms have patterns
- `loop {...}`
- `break`
- use `match` to handle errors
- `Err(_)`
# Variables and Mutability
- variables immutable by default
- constants must have type annotations
- constants cannot be computed
- `let` shadows
- same name, multiple types
# Data Types
- scalars and compound
- `isize`, `usize` for architecture
- 2's complement integers
- `_ _` separators
- overflow leads to wrapping
- IEEE 754 floats
- `bool`, `true`, `false`
- `char` is four-byte Unicode
- tuples have fixed length
- `tuple.0`
- `tuple.1 ...`
- arrays are singletype and fixed-length, on stack
- `array[index]`
# Functions
- snake case
- no declaration order
- parameters v. arguments
- must annotate types
- statement v. expression
- final expression returns
- return for explicit
# Control Flow
- branch
- arms
- it is an expression
- `loop`, `break`
- `break` can return a value
- `for ... in ...`
# What is ownership?
- central feature
- compile-time memoray management
- stack v. heap
1.  each value has an owner variable
2.  there can only be one owner at once
3.  when owner goes out of scope, value is dropped
- string literals immutable
- `String` mutable, on heap
- `String::from("hello")`
- auto drop calls
- RAII
- move makes shallow copy and invalidates source reference
- use clone for deep copy
- `Copy` trait
- passing to function moves or copies
- returning can, too
# References and Borrowing
- `&String`
- reference w/o ownership
- `*` dereference operator
- borrowing
- cannot mutate
- mutable reference
- only one mutable reference at a time
- no data races
- can't mix mutable and immutable references
- dangling references impossible
- can have either:
  - one mutable reference
  - any number of immutable references
- references always valid
# The Slice Type
- slices do not have ownership
- tracking index start and end
- `&s[0..5]` or `&s[..5]`
- string literals are slices
- take `&str` as parameter
- array slices, too
# Structs
- tuples with names
- fields
- instance
- `user.email`
- immutable by default
- field init shorthand
- struct update syntax
- `..user`
- tuple structs
- unit-like structs have no fields
- unit type `()`
- need lifetimes to reference other owned
# Example with Structs
- `print!` expects `Display`
- `{:?}` debug format
- `$[derive(Debug)]`
- `{#?}`
# Method Syntax
- functions in context of struct
- first param `self`
- `impl Rectangle { fn area (&self) -> u32 ... }`
- `rect1.area()`
- methods can own and borrow
- automatic ref. and deref. of method invocations
- implicit borrowing for method receivers
- associated functions don't take self, e.g. `String::from`
- can use many impl blocks
# Defining an Enum
- `enum IPAddrKind { V4, V6 }`
- `IPAddrKind::V4`
- `enum IPAddrKind { V4(u8,u8,u8,u8) ... }`
- `Option`: something or nothing
- no null
- `enum Option<T> { Some(T), None, }`
- [ ] Read Option doc.
- `match`
# match
- patterns
- arms
- `Some(i) => Some(i + 1)`
- matches must be exhaustive
- `_` for default
# if let
- match only one pattern
- `if let pattern = expression { ... }`
- syntactic sugar
# Packages and Crates
- package: crates
- create: tree of module, lib, or exe
- module
- paths
- crate: binary or library
- crate root
- package has `Cargo.toml`
- `src/main.rs`
- `src/lib.rs`
# Modules
- modules control privacy
- `mod`
- module tree
- sibling
- child
- parent
# Paths
- absolute
- relative: `self`, `super`, `identifier`
- `::` separator
- create::
- privacy boundary
- `pub mod`
- `pub fn`
- ancestors only
- `super` is like file system `...`
- `pub struct`
- `pub enum`
# `use`
- `use` brings module into scope
- like symlink
- use functions' parent module (idiomatic)
- `as` to alias
- `pub use`
- `std`
- `cargo`
- `use std::{cmp::Ordering, io};`
- `use std::collections::*;`
# Files
- `mod front_of_house;`
- `pub mod hosting;`
# Vectors
- `Vec<t>`
- same type
- `Vec::new()`
- `vec! macro`
- `v.push(x)`
- dropping vector drops its contents
- `&v[n]`
- `v.get(n)` can return None w/o panic
- `for i in &v { ... }`
- `for i in &mut v { ... }`
- use enums to store different types
# Strings
- `data.to_string()`
- `"".to_string()`
- `.push_str("x")`
- `s.push("1")`
- `+` uses add
- deref coercion
- `format!`
- `String` wraps `Vec<u8>`
- `s.len()` in bytes
- graphemes
- range slicing can cause panics
- `s.chars()`
- `s.bytes()`
- "strings are complicated"
# Hash Maps
- `HashMap<K,V>`
- `::new()`
- `.insert(k,v)`
- heap
- homogeneous
- `zip` and `collect`
- `Copy` trait? copied
- owned? moved
- `.get(k)`
- `Option<&V>`
- `for (key, value) in &hm`
- `hm.entry(k).or_insert(v)`
- default: `crypto.hash`
- siphash
- `BuildHasher` trait
# Errors
- recoverable and unrecoverable
- no exceptions
- `Result<T,E>`
- `panic!`
# `panic!`
- prints, unwinds stack, quits
- `Cargo.toml`:
  ```
  [profile.release]
  panic = 'abort'`
  ```
- `RUST_BACKTRACKE` env var
- `--release` flag
# Result
- `OK(T)`
- `Err(E)`
- `File::open()`
- `Err(error) => match error.kind()`
- `.unwrap_or_else(closure)`
- `.unwrap()` calls `panic!`
- propagate errors by returning
- `?` operator
- `From` `&.from`
# To `panic!` or not to `panic!`
- default: `Result`
- unwrap & expect for prototype
- bad state
- use types
# Traits
- compare to interfaces
- methods
- `pub trait Summary { fn summarize (&self) -> String ; }`
- `impl Summary for NewsArticle { .. }`
- can't implement external traits on external types
- coherence
- orphan rule
- default method implementations
- `pub fn notify (item: impl Summary) { ... }`
- sugar
- `pub fn notify<T:Summary>(item: T)`
- 
  ```
  `pub fn notify(item: impl Summary + Display) <T:Summary + Display> ...`
  where T: Display + Clone
        U: Clone + Debug
  ```
- `fn ... -> impl Summary`
- conditional method implementations
# Generics
- `fn largest<T>(list: &[T]) -> T`
- `struct Point<T>`
- `enum Option<T>`
- `impl<T> Point<T>`
- monomorphization at compile time
# Lifetimes
- every reference has lifetime, a scope
- mostly implied
- distinctive feature
- prevent dangling references
- "does not live long enough"
- borrow checker
- annotations
- `'a`
- `&'a i32`
- `fn longest<'a>(x: &'a str, y:'a str) -> &a'str`
- live at least as long as `'a`
- also in structs
- elision
- input lifetimes, output lifetimes
- methods
- `'static`: entire program run
# Tests
- function with test attribute
- `#[test]`
- `cargo test`
- `assert_eq!`
- `panic!`
- `assert!`
- `mod tests with user super::*;`
- `assert_eq!`
- `assert_ne!`
- `#[should_panic]`
- tests can return `Result<T,E>`
# Controlling How Tests Are Run
- default: parallel, threaded
- `cargo test -- --test-threads=1`
- captures stdout
- `--nocapture`
- `cargo test <name>`
- filtering pattern
- `#[ignore]`
- `--ignored`
# Test Organization
- unit & integration
- unit:
  - `mod tests`
  - `#[cfg(test)]`
- integration:
  - outside of library
  - tests directory
  - each file compiled as crate
  - `tests/common/mod.rs`
- binaries: `src/main.rs` calls `src/lib.rs`
# Closures
- capture from scope
- `|var| { ... }`
- don't require type annotation, but can
- traits:
  - `Fn`
  - `FnMut`
  - `FnOnce`
- `let equal_to_x = |x| z == x;`
# Iterators
- lazy
- `vl.iter()`
- `for val in inter { ... }`
- `Iterator` trait with next method
- "consuming adaptors" call `next`
- "iterator adaptors", e.g. map
- `collect()`
- `filter()`
- `zip()`
- `skip()`
- `sum()`
# Release Profiles
- different configurations
- compilation
- dev and release
- opt-level
# Publishing
- doc comments: `///`
- Markdown within
- `cargo doc`
- `rustdoc`
- `#Examples`
- `#Panics`
- `#Errors`
- `#Safety`
- `cargo test` runs examples
- `//!` adds doc to container, e.g. in crate root
- `cargo doc --open`
- `Cargo.toml`
- `cargo publish`
- `cargo yank`
# Cargo Workspaces
- one `Cargo.lock` for multiple packages
- 
  ```
  [workspace]
  members = ["test",]
  ```
- ...
# Smart Pointers
- pointers with metadata
- e.g. ref. counting
- e.g. `String`, `Vec<T>`
- usually implemented as structs
- traits: `Deref`, `Drop`
- common:
  - `Box<T>` for heap
  - `Rc<T>` for ref counting
  - `Ref<T>` & `RefMut<T>` via `RefCell<T>` enforce borrowing at runtime
# `Box<T>`
- heap storage
- type of unknown size at compile time
- ensure large data won't be copied
- own value, only want to be sure implements trait
- recursive types
- 
  ```
  enum List {
    Const (i32, Box<List>),
    Nil,
  }
  ```
# `Deref` Trait
- operator: `*`
- deref coercion
- mutable to immutable conversion
# `Drop` Trait
- behavior when goes out of scope
- std::mem::drop
# `Rc<T>`
- reference counting
- single-threaded only
# `RefCell<T>`
- interior mutability: mutable even with immutable references
- unsafe wrapped in safe API
- e.g. test double, mock objects
# Reference Cycle Leaks
- difficult, but not impossible
- e.g. `Rc<T>` and `RefCell<T>`
# Threads
- process
- threads
- 1:1
- green threads (M:N)
- runtime
- stdlib: 1:1 only
- `std::thread`
- `.join()`
- `move || {...}`
# Message Passing
- Go approach
- channel
- std::sync::mpsc
- "multi-producer, single-consumer"
- transmitter
- receiver
- `tx`
- `rx`
- `.send()`
- `.recv()`
- `.clone()`
# Shared State
- mutex
- `std::sync::Mutex`
- `.lock()`
- atomic reference counting
- `std::sync::Atomic`
- `Arc<T>`
# Sync and Send
- `std::marker`
- Send: ownership can be transferred between threads
- Sync: safe to reference from >=1 threads
- Don't usually have to implement manually.
