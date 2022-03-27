# Selection Control Structures

### Table of content

- [1. Boolean Value](#1-boolean-value)
- [2. Conditional Statements](#2-conditional-statements)
- [3. Short-circuit Evaluation](#3-short-circuit-evaluation)
- [4. "Dangling else” Problem](#4-dangling-else-problem)
- [5. switch or case statements; break; continue]()

## 1. Boolean Value

The `bool` represents a value, which could only be either `true` or `false`. If you **cast** a `bool` into an `integer`, `true` will be `1` and `false` will be `0`.

An example of `bool` cast to `integer` type:
```rust
assert_eq!(true as i32, 1);
assert_eq!(false as i32, 0);
```

An object with the boolean type has a size and alignment of 1 each. The value `false` has the bit pattern `0x00` and the value `true` has the bit pattern `0x01`. It is undefined behavior for an object with the boolean type to have any other bit pattern.

`bool` implements various traits, such as `BitAnd`, `BitOr`, `Not`, etc., which allow us to perform boolean operations using `&`, `|` and `!`.

`if` requires a `bool` value as its conditional. `assert!`, which is an important macro in testing, checks whether an expression is `true` and panics if it isn’t.

```rust
let bool_val = true & false | false;
assert!(!bool_val);
```

Example:
```rust
let praise_the_borrow_checker = true;

// using the `if` conditional
if praise_the_borrow_checker {
    println!("oh, yeah!");
} else {
    println!("what?!!");
}

// ... or, a match pattern
match praise_the_borrow_checker {
    true => println!("keep praising!"),
    false => println!("you should praise!"),
}
```

## 2. Conditional Statements

### 2.1. if expresssion

> Syntax
>
>*IfExpression* :
>
>`if` [*Expression*](https://doc.rust-lang.org/reference/expressions.html) <sub>*except struct expression*</sub> [*BlockExpression*](https://doc.rust-lang.org/reference/expressions/block-expr.html)
>
>(`else` ( *BlockExpression* | *IfExpression* | *IfLetExpression* ) )?

An `if` expression is a conditional branch in program control. The syntax of an `if` expression is a *condition operand*, followed by a *consequent block*, any number of `else if` conditions and blocks, and an optional trailing `else` block. 

The *condition operands* must have the `boolean type`. 

- If a *condition operand* evaluates to `true`, the consequent block is *executed* and any subsequent `else if` or `else` block is *skipped*. 
- If a *condition operand* evaluates to `false`, the consequent block is *skipped* and any subsequent `else if` condition is *evaluated*. 
- If all `if` and `else if` conditions evaluate to `false` then any `else` block is *executed*. An if expression evaluates to the *same value* as the *executed block*, or () if no block is evaluated. An if expression must have the same type in all situations.

```rust
if x == 4 {
    println!("x is four");
} else if x == 3 {
    println!("x is three");
} else {
    println!("x is something else");
}

let y = if 12 * 15 > 150 {
    "Bigger"
} else {
    "Smaller"
};
assert_eq!(y, "Bigger");
```

We can place if statements inside other if statements, a technique known as nesting.

```rust
let number = 5;

    if number < 10 {
        // if outer condition proves
        // true evaluate the inner if
        if number > 0 {
            println!("Inner if statement");
        }
    }
```
### 2.2. match expressions
>Syntax

>*MatchExpression* :
>
>`match` *Scrutinee* `{`
> 
>[*InnerAttribute*](https://doc.rust-lang.org/reference/attributes.html)*
> 
>MatchArms<sup>?</sup>
> 
>`}`

>*Scrutinee* :
>
>[*Expression*](https://doc.rust-lang.org/reference/expressions.html) <sub>except struct expression</sub>

>*MatchArms* :
>
>( *MatchArm* `=>` ( [*ExpressionWithoutBlock*](https://doc.rust-lang.org/reference/expressions.html) `,` | [*ExpressionWithBlock*](https://doc.rust-lang.org/reference/expressions.html) `,`<sup>?</sup> ) )*
>
>*MatchArm* `=>` [*Expression*](https://doc.rust-lang.org/reference/expressions.html) `,`<sup>?</sup>

>*MatchArm* :
> 
>[*OuterAttribute*](https://doc.rust-lang.org/reference/attributes.html)* [*Pattern*](https://doc.rust-lang.org/reference/patterns.html) MatchArmGuard?

>MatchArmGuard :
>
>`if` [Expression](https://doc.rust-lang.org/reference/expressions.html)

A `match` expression branches on a *pattern*. The exact form of matching that occurs depends on the *pattern*. A `match` expression has a *scrutinee expression*, which is the value to compare to the *patterns*. The *scrutinee expression* and the *patterns* must have the *same type*.

A `match` behaves differently depending on whether or not the scrutinee expression is a *place expression* or *value expression*. 
- If the *scrutinee expression* is a *value expression*, it is first evaluated into a temporary location, and the resulting value is *sequentially compared* to the *patterns* in the arms until a match is found. The first arm with a matching pattern is chosen as the branch target of the `match`, any variables bound by the pattern are assigned to local variables in the arm's block, and control enters the block.

- When the *scrutinee expression* is a *place expression*, the match *does not allocate* a temporary location; however, a by-value binding may copy or move from the memory location. When possible, it is preferable to match on place expressions, as the lifetime of these matches inherits the lifetime of the place expression rather than being restricted to the inside of the match.

An example of a match expression:

```rust
let x = 1;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    4 => println!("four"),
    5 => println!("five"),
    _ => println!("something else"),
}
```
Variables bound within the pattern are scoped to the match guard and the arm's expression. The [binding mode](https://doc.rust-lang.org/reference/patterns.html#binding-modes) (move, copy, or reference) depends on the pattern.

Multiple match patterns may be joined with the `|` operator. Each pattern will be tested in *left-to-right* sequence until a successful match is found.

```rust
let x = 9;
let message = match x {
0 | 1  => "not many",
2 ..= 9 => "a few",
_      => "lots"
};

assert_eq!(message, "a few");

// Demonstration of pattern match order.
struct S(i32, i32);

match S(1, 2) {
S(z @ 1, _) | S(_, z @ 2) => assert_eq!(z, 1),
_ => panic!(),
}
```

### 2.3. if let expressions

>Syntax
>
>*IfLetExpression* :
>
>`if` `let` [Pattern](https://doc.rust-lang.org/reference/patterns.html) `=` [*Scrutinee*](https://doc.rust-lang.org/reference/expressions/match-expr.html) <sub>except lazy boolean operator expression</sub> [*BlockExpression*](https://doc.rust-lang.org/reference/expressions/block-expr.html)
>
>(`else` ( *BlockExpression* | *IfExpression* | *IfLetExpression* ) )?

An `if let` expression is semantically similar to an `if` expression but in place of a condition operand it expects the keyword `let` followed by a pattern, an `=` and a [scrutinee](https://doc.rust-lang.org/reference/glossary.html#scrutinee) operand. If the value of the scrutinee matches the pattern, the corresponding block will *execute*. Otherwise, flow proceeds to the following `else` block if it exists. Like `if` expressions, `if let` expressions have a value determined by the block that is evaluated.

```rust
fn main() {

    let number = if true {
        1
    } else {
        2
    };

    println!("Number: {}", number);
}
```
In the example above, the if statement will prove `true` so the value assigned to the number variable is `1`.

```rust
fn main() {

    let is_birthday = true;

    let message = if is_birthday {
        "Happy birthday"
    } else {
        "No cake for you"
    };

    println!("{}", message); // error because message assigned different type
}
```
As we know, a variable can only hold one type of value. If we try to assign different types of values to it through the if statement, the compiler will raise an error.

Another example:
```rust
fn main() {
let dish = ("Ham", "Eggs");

// this body will be skipped because the pattern is refuted
if let ("Bacon", b) = dish {
    println!("Bacon is served with {}", b);
} else {
    // This block is evaluated instead.
    println!("No bacon will be served");
}

// this body will execute
if let ("Ham", b) = dish {
    println!("Ham is served with {}", b);
}

if let _ = 5 {
    println!("Irrefutable patterns are always true");
}
}
```

`if` and `if let` expressions can be intermixed:

```rust
let x = Some(3);
let a = if let Some(1) = x {
    1
} else if x == Some(2) {
    2
} else if let Some(y) = x {
    y
} else {
    -1
};
assert_eq!(a, 3);
```

An `if let` expression is equivalent to a [match expression](#22-match-expressions) as follows:
```rust
if let PATS = EXPR {
    /* body */
} else {
    /*else */
}
```
is equivalent to
```rust
match EXPR {
    PATS => { /* body */ },
    _ => { /* else */ },    // () if there is no else
}
```





## 3. Short-circuit Evaluation

## 4. "Dangling else” Problem