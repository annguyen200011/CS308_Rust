# Objects and Classes

### Table of content
- [1. Characteristics of Object-Oriented Languages](#1-characteristics-of-object-oriented-languages)
  - [1.1. Objects Contain Data and Behavior](#11-objects-contain-data-and-behavior)
  - [1.2. Encapsulation that Hides Implementation Details](#12-encapsulation-that-hides-implementation-details)
  - [1.3. Inheritance and Polymorphism](#13-inheritance-and-polymorphism)
- [2. The Rust Standard Library](#2-the-rust-standard-library)
- [Reference](#reference)


## 1. Characteristics of Object-Oriented Languages
Rust is influenced by many programming paradigms, including OOP

### 1.1. Objects Contain Data and Behavior

> Object-oriented programs are made up of objects. An object packages both data and the procedures that operate on that data. The procedures are typically called methods or operations.

Using this definition, Rust is object oriented: `structs` and `enums` have data, and `impl` blocks provide _methods_ on `structs` and `enums`. Even though `structs` and `enums` with methods aren’t called objects, they provide the same functionality.

### 1.2. Encapsulation that Hides Implementation Details

Another aspect commonly associated with OOP is the idea of **encapsulation**, which means that the implementation details of an object aren’t accessible to code using that object. Therefore, the only way to interact with an object is through its public API; code using the object shouldn’t be able to reach into the object’s internals and change data or behavior directly. This enables the programmer to change and refactor an object’s internals without needing to change the code that uses the object.

we can use the `pub` keyword to decide which modules, types, functions, and methods in our code should be public, and by default everything else is private.

```rust
pub struct AveragedCollection {
    list: Vec<i32>,
    average: f64,
}
```

The struct is marked `pub` so that other code can use it, but the fields within the struct remain **private**. This is important in this case because we want to ensure that whenever a value is added or removed from the list, the average is also updated. We do this by implementing `add`, `remove`, and `average` methods on the struct.

```rust
impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }

    pub fn average(&self) -> f64 {
        self.average
    }

    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

The public methods `add`, `remove`, and `average` are the only ways to access or modify data in an instance of `AveragedCollection`. When an item is added to list using the `add` method or removed using the `remove` method, the implementations of each call the private `update_average` method that handles updating the `average` field as well.

### 1.3. Inheritance and Polymorphism

>_Inheritance_ is a mechanism whereby an object can inherit from another object’s definition, thus gaining the parent object’s data and behavior without you having to define them again.

Considering inheritance, the Rust programming language has no way of specifying that a struct inherits attributes or functionality from another one. So in this sense, Rust is not an object oriented programming language. However there are other solutions that the Rust language offers that are similar to (but not quite) inheritance, for example, Traits.

>A trait tells the Rust compiler about functionality a particular type has and can share with other types. We can use traits to define shared behavior in an abstract way. We can use trait bounds to specify that a generic type can be any type that has certain behavior.

Note: Traits are similar to a feature often called interfaces in other languages, although with some differences.

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

This is an example of a Trait. Just like an interface, any class or structure in the case of Rust that uses this, MUST implement it somewhere in it's own definition. A trait can have multiple methods in it's body, and anyone using it must implement them all.

The implementation would look something like this:

```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```

Although sometimes it's useful or necessary to have some default implementation for these. Which is possible.

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

If we want to use the default implementation, we would have an empty impl block.

To many people, polymorphism is synonymous with inheritance. But it’s actually a more general concept that refers to code that can work with data of multiple types. For inheritance, those types are generally subclasses.

Rust instead uses generics to abstract over different possible types and trait bounds to impose constraints on what those types must provide. This is sometimes called bounded parametric polymorphism.

To be able to explain thoroughly and well what these are, I'd have to first explain a few other topics more specific about Rust's syntax and way of doing things, so for now, know that it's possible to have some form of polymorphism in Rust, just not the way classical OOP languages do it.

## 2. The Rust Standard Library
The Rust Standard Library is the foundation of portable Rust software, a set of minimal and battle-tested shared abstractions for the broader Rust ecosystem

`std` is available to all Rust crates by default. Therefore, the standard library can be accessed in `use` statements through the path `std`, as in use `std::env`.

First of all, The Rust Standard Library is divided into a number of focused modules. These modules are the bedrock upon which all of Rust is forged, and they have mighty names like `std::slice` and `std::cmp`. Modules' documentation typically includes an overview of the module along with examples, and are a smart place to start familiarizing yourself with the library.

| Module      | uses                                                                                                                                          |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| alloc 	     | Memory allocation APIs                                                                                                                        |
| any 	       | This module implements the Any trait, which enables dynamic typing of any 'static type through runtime reflection.                            |
| array 	     | Implementations of things like Eq for fixed-length arrays up to a certain length. Eventually, we should be able to generalize to all lengths. |
| ascii 	     | Operations on ASCII strings and characters.                                                                                                   |
| char        | A character type.                                                                                                                             |
| clone       | The Clone trait for types that cannot be 'implicitly copied'.                                                                                 |
| cmp         | Functionality for ordering and comparison.                                                                                                    |
| collections | Collection types.                                                                                                                             |
| convert     | Traits for conversions between types.                                                                                                         |
| default     | The `Default` trait for types which may have meaningful default values.                                                                       |
| error       | Traits for working with Errors.                                                                                                               |
| f32         | This module provides constants which are specific to the implementation of the f32 floating point data type.                                  |
| i32         | The 32-bit signed integer type.                                                                                                               |
| num         | Additional functionality for numerics.                                                                                                        |
| str         | Unicode string slices.                                                                                                                        |
| string      | A UTF-8 encoded, growable string.                                                                                                             |
| slice       | A dynamically-sized view into a contiguous sequence, `[T]`.                                                                                   |
| vec         | A contiguous growable array type with heap-allocated contents, written Vec<T>.                                                                |

Second, implicit methods on [primitive types](https://github.com/annguyen200011/CS308_Rust/blob/main/Names%2C%20Types%20and%20Binding.md). This can be a source of confusion for two reasons:

1. While primitives are implemented by the compiler, the standard library implements methods directly on the primitive types (and it is the only library that does so), which are documented in the section on primitives.
2. The standard library exports many modules with the same name as primitive types. These define additional items related to the primitive type, but not the all-important methods.

Examples:
```rust
// deprecated way
let min = std::i32::MIN;

// intended way
let min = i32::MIN;
```

_Trait std::string::ToString_
```rust
pub trait ToString {
  fn to_string(&self) -> String;
}
```
```rust
let i = 5;
let five = String::from("5");

assert_eq!(five, i.to_string());
```

Third, the standard library defines The Rust Prelude, a small collection of items - mostly traits - that are imported into every module of every crate. The traits in the prelude are pervasive, making the prelude documentation a good entry point to learning about the library.

And finally, the standard library exports a number of standard macros (technically, not all of the standard macros are defined by the standard library - some are defined by the compiler - but they are documented here the same). Like the prelude, the standard macros are imported by default into all crates.

| Macros          | uses                                                                              |
|-----------------|-----------------------------------------------------------------------------------|
| assert          | Asserts that a boolean expression is true at runtime.                             |
| assert_eq       | Asserts that two expressions are equal to each other (using PartialEq).           |
| assert_ne       | Asserts that two expressions are not equal to each other (using PartialEq).       |
| cfg             | Evaluates boolean combinations of configuration flags at compile-time.            |
| column          | Expands to the column number at which it was invoked.                             |
| compile_error   | Causes compilation to fail with the given error message when encountered.         |
| concat          | Concatenates literals into a static string slice.                                 |
| dbg             | Prints and returns the value of a given expression for quick and dirty debugging. |
| debug_assert    | Asserts that a boolean expression is true at runtime.                             |
| debug_assert_eq | Asserts that two expressions are equal to each other.                             |
| debug_assert_ne | Asserts that two expressions are not equal to each other.                         |
| env             | Inspects an environment variable at compile time.                                 |
| panic           | Panics the current thread.                                                        |
| print           | Prints to the standard output.                                                    |
| println         | Prints to the standard output, with a newline.                                    |
| todo            | Indicates unfinished code.                                                        |
| try 	           | `Deprecated` Unwraps a result or propagates its error.                            |
| include         | Parses a file as an expression or an item according to the context.               |
| include_bytes   | Includes a file as a reference to a byte array.                                   |
| include_str     | Includes a UTF-8 encoded file as a string.                                        |

## Reference
1. https://iq.opengenus.org/oop-in-rust/
2. https://doc.rust-lang.org/book/ch17-00-oop.html
3. https://docs.w3cub.com/rust/std/index
4. 


