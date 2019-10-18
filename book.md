# Introduction
- cargo new
- Cargo.toml
- let (immutable)
- let mut (mutable)
- "associated function"
- & to reference
- references immutable by default
- enums have set of possible variants, e.g. Ok, Err
- Result types encode error info
- [dependencies]
- cargo update
- Cargo.lock
- trails define methods
- cargo doc --open
- match expressions have arms
- arms have patterns
- loop {...}
- break
- use match to handle errors
- Err(_)
# Variables and Mutability
- variables immutable by default
- constants must have type annotations
- constants cannot be computed
- let shadows
- same name, multiple types
# Data Types
- scalars and compound
- isize, usize for architecture
- 2's complement integers
- _ _ separators
- overflow leads to wrapping
- IEEE 754 floats
- bool, true, false
- char is four-byte Unicode
- tuples have fixed length
- tuple.0
- tuple.1 ...
- arrays are singletype and fixed-length, on stack
- array[index]
# Functions
- snake case
- no declaration order
- parameters v. arguments
- must annotate types
- statemnet v. expression
- final expression returns
- return for explicit
# Control Flow
- branch
- arms
- it is an expression
- loop, break
- break can retur a value
- for ... in ...
# What is ownership?
- central feature
- compile-time memoray management
- stack v. heap
1.  each value has an owner variable
2.  there can only be one owner at once
3.  when owner goes out of scope, value is dropped
- string lietrals immutable
- String mutable, on heap
- String::from("hello")
- auto drop calls
- RAII
- move makes shallow copy and invalidates source reference
- use clone for deep copy
- Copy trait
- passing to function moves or copies
- returning can, too
# References and Borrowing
- &String
- reference w/o ownership
- * dereference operator
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
- &s[0..5] or &s[..5]
- string literals are slices
- take &str as parameter
- array slices, too
# Structs
- tuples with names
- fields
- instance
- user.email
- immutable by default
- field init shorthand
- struct update syntax
- ..user
- tuple structs
- unit-like structs have no fields
- unit type ()
- need lifetimes to reference other owned
# Example with Structs
- print! expects Display
- {:?} debug format
- $[derive(Debug)]
- {#?}
# Method Syntax
- functions in context of struct
- first param self
- impl Rectangle { fn area (&self) -> u32 ... }
- rect1.area()
- methods can own and borrow
- automatic ref. and deref. of method invocations
- implicit borrowing for method receivers
- associated functions don't take self, e.g. String::from
- can use many impl blocks
# Defining an Enum
- enum IPAddrKind { V4, V6 }
- IPAddrKind::V4
- enum IPAddrKind { V4(u8,u8,u8,u8) ... }
- Option: something or nothing
- no null
- enum Option<T> { Some(T), None, }
- [ ] Read Option doc.
- match
# match
- patterns
- arms
- Some(i) => Some(i + 1)
- matches must be exhaustive
- _ for default
# if let
- match only one pattern
- if let pattern = expression { ... }
- syntactic sugar
# Packages and Crates
- package: crates
- create: tree of module, lib, or exe
- module
- paths
- crate: binary or library
- crate root
- package has Cargo.toml
- src/main.rs
- src/lib.rs
# Modules
- modules control privacy
- mod
- module tree
- sibling
- child
- parent
# Paths
- absolute
- reltive: self, super, identifier
- :: separator
- create::
- privacy boundary
- pub mod
- pub fn
- ancestors only
- super is like filesystem ...
- pub struct
- pub enum
# use
- use brings module into scope
- like symlink
- use functions' parent module (idiomatic)
- as to alias
- pub use
- std
- cargo
- use std::{cmp::Ordering, io};
- use std::collections::*;
# Files
- mod front_of_house;
- pub mod hosting;