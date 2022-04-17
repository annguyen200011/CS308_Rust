# History-and-Hello-World: Rust
![image](https://user-images.githubusercontent.com/88206341/154418337-aacad72f-c7ce-43d3-b39f-9a8a8f3c083d.png)
### Table of content
- [Overview](#overview)
  - [History](#history)
  - [Why Rust?](#why-rust)
  - [Rust is used for?](#rust-is-used-for)
  - [Learning sources](#learning-sources)
- [Setup](#setup) 
  - [Main tool](#main-tool)
  - [How to install?](#how-to-install)
  - [Run program in Intellij IDEA](#run-program-in-intellij-idea)
  - [Comments](#comments)
    - [Non-doc comments](#non-doc-comments)
    - [Doc comments](#doc-comments)
    - [Examples](#examples)
 - [Reference](#reference)  

## Overview
Name: **Rust**

### History

First appearing in 2010, Rust was designed by Graydon Hoare at Mozilla Research, with contributions from Dave Herman, Brendan Eich, and others.

Rust is a multi-paradigm, general-purpose programming language designed for performance and safety, especially safe concurrency. Rust is syntactically _similar to C++_, but can guarantee _memory safety_. Rust has been called a systems programming language and in addition to high-level features such as functional programming it also offers mechanisms for low-level memory management.

### Why Rust?
- **Performance:** Rust is blazingly fast and memory-efficient: with no runtime or garbage collector, it can power performance-critical services, run on embedded devices, and easily integrate with other languages.
- **Reliability:** Rust’s rich type system and ownership model guarantee memory-safety and thread-safety — enabling you to eliminate many classes of bugs at compile-time.
- **Productivity:** Rust has great documentation, a friendly compiler with useful error messages, and top-notch tooling — an integrated package manager and build tool, smart multi-editor support with auto-completion and type inspections, an auto-formatter, and more.

### Rust is used for?
Rust has been adopted by major software engineering companies. Rust is used for:
- Web browsers and services: Firefox, OpenDNS, Deno, Figma,...
- Operating systems: Redox, Theseus, Google Fuchia, Stratis,...
- Other notable projects and platforms: Discord, Microsoft Azure, Amazon Web Services, _Polkadot (cryptocurrency)_,...

> Note: What I like most about rust programming language is its popularity in Dapps(Decentralize Applications) programming. I plan to program blockchain applications. 

### Learning sources
- Pages: 
  - Basic
    - [The Rust Programming Language](https://doc.rust-lang.org/stable/book/)
    - [The Cargo Book](https://doc.rust-lang.org/cargo/index.html)
    - [Rust By Example](https://doc.rust-lang.org/rust-by-example/)
  - Domain Specific
    - [Command Line Applications in Rust](https://rust-cli.github.io/book/index.html)
    - [Rust and WebAssembly](https://rustwasm.github.io/docs/book/#how-to-read-this-book)
    - [The Embedded Rust Book](https://doc.rust-lang.org/stable/embedded-book/)
    - [An Experimental Course on Operating Systems](https://cs140e.sergio.bz/)
  - Advance
    - [Crate std](https://doc.rust-lang.org/std/index.html)
    - [The Rust Reference](https://doc.rust-lang.org/reference/index.html)
    - [The Rust Unstable Book](https://doc.rust-lang.org/nightly/unstable-book/index.html)
    - [The Rustonomicon](https://doc.rust-lang.org/nomicon/)
    - [The Little Book of Rust Macros](https://veykril.github.io/tlborm/introduction.html)
    - [A Guide to Porting C/C++ to Rust](https://locka99.gitbooks.io/a-guide-to-porting-c-to-rust/content/)
    - [The Rust FFI Omnibus](https://jakegoulding.com/rust-ffi-omnibus/)
    - [Lifetime Annotations](https://www.youtube.com/watch?v=rAl-9HwD858&list=PLqbS7AVVErFiWDOAVrPt7aYmnuuOLYvaO&t=0s)
  - Hands On Practice
    - https://github.com/rust-lang/rustlings
    - https://exercism.org/tracks/rust/exercises
    - https://github.com/rust-unofficial/awesome-rust
- Course: [Rustling course](https://github.com/rust-lang/rustlings/)
- Dapps Developers: [Ethereum](https://ethereum.org/en/developers/docs/programming-languages/rust/), [Solana](https://docs.solana.com/developing/on-chain-programs/developing-rust), [Polkadot](https://polkadot.network/technology/), [Near](https://docs.near.org/docs/develop/contracts/rust/intro),...
- Blog: [educative.io](https://www.educative.io/blog/rust-tutorial-from-scratch)
- Youtube: [dcode](https://www.youtube.com/watch?v=vOMJlQ5B-M0&list=PLVvjrrRCBy2JSHf9tGxGKJ-bYAN_uDCUL), [Traversy Media](https://www.youtube.com/watch?v=zF34dRivLOw), [Blockchain cùng Vũ](https://www.youtube.com/watch?v=k9dM0SiX9iU&list=PLFnEYduGTiXE2ejxmzTIraP2feI-pmeuw), [Jon Gjenset](https://www.youtube.com/channel/UC_iD0xppBwwsrM9DegC5cQQ), 

### Next reading
1. [Names, Types and Binding](https://github.com/annguyen200011/CS308_Rust/blob/main/Names%2C%20Types%20and%20Binding.md#names-types-and-binding)
2. [Selection Control Structures](https://github.com/annguyen200011/CS308_Rust/blob/main/Selection%20Control%20Structures.md)
3. [Loops and Subroutines](https://github.com/annguyen200011/CS308_Rust/blob/main/Loops%20and%20Subroutines.md)

## Setup

### Main tool
1. rustc: The complier which takes your Rust code and compiles it into binary (machine readable code)
2. rustup: The command line utility to install and update Rust
3. cargo: The Rust build system and package manager

### How to install?
1. Install Programing Enviroment: [Visual Studio C++ Build tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) 
2. Install [Rust](https://www.rust-lang.org/tools/install)
3. Install [Intellij IDEA](https://www.jetbrains.com/idea/) and [Intellij Rust Plugin](https://www.jetbrains.com/rust/)

### Run program in Intellij IDEA
1. Create new project: Go to **File** | **New** | **Project** (File | New Project) or click **New project** on the welcome screen.
2. Select **Rust** from the list in the left-hand pane.
3. Choose the template to used, specify the project name, location, and toolchain parameters:
![image](https://user-images.githubusercontent.com/88206341/154417869-3540cdb4-8be8-4d91-8ea9-7f66bc66d4ae.png)

4. Click **Create** when ready, and the IDE will generate a ready-to-go stub project:
![image](https://user-images.githubusercontent.com/88206341/154417918-4ab4f2ae-7327-4f1f-9e83-6de50179ff17.png)


### Comments

#### Non-doc comments
Comments in Rust code follow the general C++ style of line (`//`) and block (`/* ... */`) comment forms. Nested block comments are supported.

Non-doc comments are interpreted as a form of whitespace.

#### Doc comments
Line doc comments beginning with exactly three slashes (`///`), and block doc comments (`/** ... */`), both inner doc comments, are interpreted as a special syntax for `doc` attributes. That is, they are equivalent to writing `#[doc="..."]` around the body of the comment, i.e., /// Foo turns into `#[doc="Foo"]` and `/** Bar */` turns into #`[doc="Bar"]`.

Line comments beginning with `//!` and block comments `/*! ... */` are doc comments that apply to the parent of the comment, rather than the item that follows. That is, they are equivalent to writing `#![doc="..."]` around the body of the comment. `//!` comments are usually used to document modules that occupy a source file.

Isolated CRs (`\r`), i.e. not followed by LF (`\n`), are not allowed in doc comments.

#### Examples

```rust

#![allow(unused_variables)]
fn main() {
//! A doc comment that applies to the implicit anonymous module of this crate

pub mod outer_module {

    //!  - Inner line doc
    //!! - Still an inner line doc (but with a bang at the beginning)

    /*!  - Inner block doc */
    /*!! - Still an inner block doc (but with a bang at the beginning) */

    //   - Only a comment
    ///  - Outer line doc (exactly 3 slashes)
    //// - Only a comment

    /*   - Only a comment */
    /**  - Outer block doc (exactly) 2 asterisks */
    /*** - Only a comment */

    pub mod inner_module {}

    pub mod nested_comments {
        /* In Rust /* we can /* nest comments */ */ */

        // All three types of block comments can contain or be nested inside
        // any other type:

        /*   /* */  /** */  /*! */  */
        /*!  /* */  /** */  /*! */  */
        /**  /* */  /** */  /*! */  */
        pub mod dummy_item {}
    }

    pub mod degenerate_cases {
        // empty inner line doc
        //!
    
        // empty inner block doc
        /*!*/

        // empty line comment
        //
        
        // empty outer line doc
        ///
        
        // empty block comment
        /**/

        pub mod dummy_item {}

        // empty 2-asterisk block isn't a doc block, it is a block comment
        /***/

    }

    /* The next one isn't allowed because outer doc comments
       require an item that will receive the doc */

    /// Where is my item?
  mod boo {}
}
```

## Reference
1. https://en.wikipedia.org/wiki/Rust_(programming_language)
2. https://www.rust-lang.org/
3. https://www.rust-lang.org/tools/install
4. https://plugins.jetbrains.com/plugin/8182-rust/docs/rust-quick-start.html
5. https://doc.rust-lang.org/reference/comments.html
6. https://www.youtube.com/watch?v=NufU_fRpStA
