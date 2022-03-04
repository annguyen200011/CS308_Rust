# Names, Types and Binding

### Table of content
- [1. Keywords & Reserved Words](#1-keywords--reserved-words)
- [2. Naming requirements for variables](#2-naming-requirements-for-variables)
- [3. Naming conventions](#3-naming-conventions)
  - [3.1. Mutability](#31-mutability)
  - [3.2. Data Type](#32-data-type)
    - [3.2.1. Scalar Types](#321-scalar-types) 
      - [3.2.1.1.Integer Types](https://github.com/annguyen200011/CS308_Rust/new/main#3211-integer-types) 
      - 
    - [3.2.2. ]()
- [4. Coding examples]()

## 1. Keywords & Reserved Words
### Keywords Currently in Use
The following keywords currently have the functionality described.

- `as` - perform primitive casting, disambiguate the specific trait containing an item, or rename items in use and extern crate statements
- `async` - return a Future instead of blocking the current thread
- `await` - suspend execution until the result of a Future is ready
- `break` - exit a loop immediately
- `const` - define constant items or constant raw pointers
- `continue` - continue to the next loop iteration
- `crate` - link an external crate or a macro variable representing the crate in which the macro is defined
- `dyn` - dynamic dispatch to a trait object
- `else` - fallback for if and if let control flow constructs
- `enum` - define an enumeration
- `extern` - link an external crate, function, or variable
- `false` - Boolean false literal
- `fn` - define a function or the function pointer type
- `for` - loop over items from an iterator, implement a trait, or specify a higher-ranked lifetime
- `if` - branch based on the result of a conditional expression
- `impl` - implement inherent or trait functionality
- `in` - part of for loop syntax
- `let` - bind a variable
- `loop` - loop unconditionally
- `match` - match a value to patterns
- `mod` - define a module
- `move` - make a closure take ownership of all its captures
- `mut` - denote mutability in references, raw pointers, or pattern bindings
- `pub` - denote public visibility in struct fields, impl blocks, or modules
- `ref` - bind by reference
- `return` - return from function
- `Self` - a type alias for the type we are defining or implementing
- `self` - method subject or current module
- `static` - global variable or lifetime lasting the entire program execution
- `struct` - define a structure
- `super` - parent module of the current module
- `trait` - define a trait
- `true` - Boolean true literal
- `type` - define a type alias or associated type
- `union` - define a union and is only a keyword when used in a union declaration
- `unsafe` - denote unsafe code, functions, traits, or implementations
- `use` - bring symbols into scope
- `where` - denote clauses that constrain a type
- `while` - loop conditionally based on the result of an expression

### Keywords Reserved for Future Use
The following keywords do not have any functionality but are reserved by Rust for potential future use.

- `abstract`
- `become`
- `box`
- `do`
- `final`
- `macro`
- `override`
- `priv`
- `try`
- `typeof`
- `unsized`
- `virtual`
- `yield`
## 2. Naming requirements for variables
Basic Rust naming conventions are described in [RFC 430](https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md).
In general, Rust tends to use UpperCamelCase for "type-level" constructs (types and traits) and snake_case for "value-level" constructs. More precisely:
| Item | Convention |
| ---- | ---------- |
| Crates | `snake_case` (but prefer single word) |
| Modules | `snake_case` |
| Types | `UpperCamelCase` |
| Traits | `UpperCamelCase` |
| Enum variants | `UpperCamelCase` |
| Functions | `snake_case` |
| Methods | `snake_case` |
| General constructors | `new` or `with_more_details` |
| Conversion constructors | `from_some_other_type` |
| Local variables | `snake_case` |
| Static variables | `SCREAMING_SNAKE_CASE` |
| Constant variables | `SCREAMING_SNAKE_CASE` |
| Type parameters | concise `UpperCamelCase`, usually single uppercase letter: `T` |
| Lifetimes | short, lowercase: `'a` |

### Fine points

In `UpperCamelCase`, acronyms count as one word: use `Uuid` rather than `UUID`.  In `snake_case`, acronyms are lower-cased: `is_xid_start`.

In `UpperCamelCase` names multiple numbers can be separated by a `_` for clarity: `Windows10_1709` instead of `Windows101709`.

In `snake_case` or `SCREAMING_SNAKE_CASE`, a "word" should never consist of a single letter unless it is the last "word". So, we have `btree_map` rather than `b_tree_map`, but `PI_2` rather than `PI2`.

## 3. Naming conventions
### 3.1. Mutability
Variable bindings are immutable by default, but this can be overridden using the `mut` modifier.
```rust
fn main() {
    let _immutable_binding = 1;
    let mut mutable_binding = 1;

    println!("Before mutation: {}", mutable_binding);

    // Ok
    mutable_binding += 1;

    println!("After mutation: {}", mutable_binding);

    // Error!
    _immutable_binding += 1;
    // FIXME ^ Comment out this line
}
```

Like immutable variables, constants are values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables.
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

### 3.2. Data type
Every value in Rust is of a *certain data* type, which tells Rust what kind of data is being specified so it knows how to work with that data. We’ll look at two data type subsets: scalar and compound.
Rust is a statically typed language, which means that it must know the types of all variables at compile time.

#### 3.2.1. Scalar Types
A scalar type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters

##### 3.2.1.1. Integer Types
An integer is a number without a fractional component.
| Length |	Signed |	Unsigned |
| ------ | ------- | --------- |
| 8-bit |	`i8` |	`u8` |
| 16-bit |	`i16` |	`u16` |
| 32-bit |	`i32` |	`u32` |
| 64-bit |	`i64` |	`u64` |
| 128-bit |	`i128` |	`u128` |
| arch |	`isize` |	`usize` |

Each variant can be either signed or unsigned and has an explicit size. Signed and unsigned refer to whether it’s possible for the number to be negative

Integer Literals in Rust

| Number literals |	Example |
| --------------- | ------- |
| Decimal |	`98_222` |
| Hex |	`0xff` |
| Octal |	`0o77` |
| Binary |	`0b1111_0000` |
| Byte (u8 only) |	`b'A'` |

##### 3.2.1.2. Floating-Point Types


## 4. Coding examples

## Reference
- https://doc.rust-lang.org/stable/book/appendix-01-keywords.html
- https://rust-lang.github.io/api-guidelines/naming.html
