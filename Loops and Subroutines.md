# Loops and Subroutines

### Table of content

- [1. Loop](#1-loop)
   - [1.1. loop](#11-loop)
     - [1.1.1. Nesting and labels](#111-nesting-and-labels)
     - [1.1.2. Returning from loops](#112-returning-from-loops)
   - [1.2. while](#12-while)
   - [1.3. for](#13-for)
     - [1.3.1. for and range]()
     - [1.3.2. for and iterators]()
- [2. Function]()
   - 2.1. Syntax
   - 2.2. Rules
- [3. Recursion]()
- [4. Multiple Parameters]()
- [5. Return]() 
- [6. Pass by]() 

## 1. Loop

### 1.1. loop
Rust provides a `loop` keyword to indicate an infinite loop.

The `break` statement can be used to exit a loop at anytime, whereas the `continue` statement can be used to skip the rest of the iteration and start a new one.

```rust
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");

    // Infinite loop
    loop {
        count += 1;

        if count == 3 {
            println!("three");

            // Skip the rest of this iteration
            continue;
        }

        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");

            // Exit this loop
            break;
        }
    }
}
```

#### 1.1.1. Nesting and labels
It's possible to `break` or `continue` outer loops when dealing with nested loops. In these cases, the loops must be annotated with some 'label, and the label must be passed to the break/continue statement.

```rust
fn main() {
    'outer: loop {
        println!("Entered the outer loop");

        'inner: loop {
            println!("Entered the inner loop");

            // This would break only the inner loop
            //break;

            // This breaks the outer loop
            break 'outer;
        }

        println!("This point will never be reached");
    }

    println!("Exited the outer loop");
}
```

#### 1.1.2. Returning from loops

One of the uses of a `loop` is to retry an operation until it succeeds. If the operation returns a value though, you might need to pass it to the rest of the code: put it after the `break`, and it will be returned by the `loop` expression.

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    assert_eq!(result, 20);
}
```

### 1.2. while

The `while` keyword can be used to run a loop while a condition is true.

```rust
fn main() {
    // A counter variable
    let mut n = 1;

    // Loop while `n` is less than 101
    while n < 101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }

        // Increment counter
        n += 1;
    }
}
```

### 1.3. for

#### 1.3.1. for and range

The `for in` construct can be used to iterate through an `Iterator`. One of the easiest ways to create an iterator is to use the range notation `a..b`. This yields values from `a` (inclusive) to `b` (exclusive) in steps of one.

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

Alternatively, `a..=b` can be used for a range that is inclusive on both ends. The above can be written as:

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..=100 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```


#### 1.3.2. for and iterators

The `for` in construct is able to interact with an `Iterator` in several ways. As discussed in the section on the Iterator trait, by default the `for` loop will apply the `into_iter` function to the collection. However, this is not the only means of converting collections into iterators.

`into_iter`, `iter` and `iter_mut` all handle the conversion of a collection into an iterator in different ways, by providing different views on the data within.

- `iter` - This borrows each element of the collection through each iteration. Thus leaving the collection untouched and available for reuse after the loop.

```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter() {
        match name {
            &"Ferris" => println!("There is a rustacean among us!"),
            // TODO ^ Try deleting the & and matching just "Ferris"
            _ => println!("Hello {}", name),
        }
    }
    
    println!("names: {:?}", names);
}   

    /* print:
    Hello Bob
    Hello Frank
    There is a rustacean among us!
    names: ["Bob", "Frank", "Ferris"]
    */
```

- `into_iter` - This consumes the collection so that on each iteration the exact data is provided. Once the collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop.
```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.into_iter() {
        match name {
            "Ferris" => println!("There is a rustacean among us!"),
            _ => println!("Hello {}", name),
        }
    }
    println!("names: {:?}", names);
    // if we use: println!("names: {:?}", names); -> error because the `names` moved due to .into_iter
}
```

- `iter_mut` - This mutably borrows each element of the collection, allowing for the collection to be modified in place.
```rust
fn main() {
    let mut names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }

    println!("names: {:?}", names);
}
//print: names: ["Hello", "Hello", "There is a rustacean among us!"]
```


#### 1.3.3. Range expression

| Production	                                                             | Syntax                                | Type                       | Range           |
|-------------------------------------------------------------------------|---------------------------------------|----------------------------|-----------------|
| RangeExpr                                                               | 	start`..`end	                          | std::ops::Range            | start ≤ x < end |
| RangeFromExpr                                                           | 	start`..`	                             | std::ops::RangeFrom	       | start ≤ x       |
| RangeToExpr	| `..`end                                 | 	std::ops::RangeTo         | 	x < end        |
| RangeFullExpr	| `..`	                                   | std::ops::RangeFull	       | -               |
| RangeInclusiveExpr	| start`..=`end| 	std::ops::RangeInclusive	 | start ≤ x ≤ end            |
| RangeToInclusiveExpr	| `..=`end	| std::ops::RangeToInclusive | 	x ≤ end                              |


