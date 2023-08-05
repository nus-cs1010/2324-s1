# C in CS1010

C is a simple and flexible language, providing programmers with many ways to achieve the same thing.

Some of these features that C provides, however, could be bug-prone.  Wei Tsang has written enough buggy programs himself and seen enough buggy programs from students.  He feels that some of these features from C are not useful for beginners (or even seasoned programmers).

Furthermore, some features in C simply encourage bad programming habits that are widely frowned upon.  Some would lead to insecure programs.  

As such, in CS1010, we _ban_ and _discourage_ the use of certain operators, functions, constructs, and features in C.

This article summarizes this list.  

## Banned in CS1010

The banned items should not be used in CS1010.  Students should use alternatives.  The teaching staff reserves the right to apply a penalty while grading the assignments and practical exams if these banned features are used.

### 1. The `++` and `--` operators.

#### Why?
- These operators lead to potential undefined behavior.  E.g., `i = i++;`
- The potential confusion is caused by the difference between `i++` and `++i`.

#### What should be used instead?
- Use `i += 1` or `i -= 1` instead of `i++` or `i--`

### 2. Skipping of curly braces for single statement conditional or loop body

#### Why?
- Could lead to dangling `else` confusion
- Easy to forget to put back the `{}` pair if the body is modified beyond a single statement

#### What should be used instead?
- Always use `{}` even if the conditional or loop body contains only a single statement.

### 3. Nested conditional operator `?:`

#### Why?
- Can get difficult to read, understand, and modify.  Example:

```
a = (x > y) ? ((y > z) ? y : z) : ((x > z) ? x : z);
```

#### What should be used
- Use nested `if-else` loop

### 4. Global variables

#### Why?
- It makes the code hard to reason about and trace, as you have no idea who will modify these variables.  For instance,  if `x` is not a global variable, we can safely assert that `x` is still 1 after calling `f()`.  If `x` is a global variable, we can no longer assert anything about `x`.

```C
x = 1;
f();
// { x == 1 }
```

#### What should be used instead
- Declare the variables as local, automatic variables, and pass them around.

### 5. Using `int` and `short`

#### Why?
- C standard guarantees that both `short` and `int` are at least 16 bits, which limits its guaranteed range to only -32,768 to 32,767.  This is too small for many purposes.
- We are not concerned about memory usage in CS1010.  If we do want to have precise control over memory, we should be anyway using the types from `stdint.h`.

#### What should be used instead
- `long`, which is guaranteed to be at least 32 bits.

#### Exception
- If a function from a C library calls for the use of `int` and offers no `long` alternative, then we have to use `int`.

### 6. The type `float`

#### Why?
- Not enough precision.  It will cause floating-point errors.

#### What should be used instead
- `double`

#### Exception
- If a function from a C library calls for the use of `float` and offers no `double` alternative, then we have to use `float`.

### 7. Using integer values for true/false

#### Why?
- Confusing and error-prone

#### What should be used instead
- Use the `bool` type, and the values `true` and `false`.


### 8. `goto`
#### Why?
- makes the logical flow of the code hard to follow and trace

#### What should be used instead
- combinations of conditionals and loops

----

## Discouraged in CS1010

These are things that are not strictly banned, but their usage is discouraged.  Students should use them only if they know very well what they are doing.  Use at own perils.

### 1. `printf` and `scanf` Functions

#### Why?
- Using the wrong format modifier for `printf` could lead to strange results
- Using the wrong format modifier for `scanf` could lead to memory corruption
- Need to look up what is the right format modifier to use
- Need to pre-allocate memory for `scanf` of strings
- `scanf` is not secure
- `scanf` is not a pure function.  Prefers students to learn about the concept of pure functions first.
- etc. etc.

#### What should be used instead
- The CS1010 I/O library

### 2. `switch` Statements

#### Why?
- Bug prone (missing `break` would cause the case to fall through)
- Only works on ordinal types.

#### What should be used instead
- `if`-`else` statements

### 3. `break` and `continue` Statements

#### Why?
- Using `break` and `continue` complicates the flow of a loop, marks it harder to reason about the correctness of the loop, and is, therefore, bug-prone.  

#### What should be used instead
- Simple loops with a single entry and a single exit point.  Use flag variables to indicate special conditions to exit or continue with the loop.

### 4. Skipping parenthesis

#### Why?
- Parenthesis makes it clear to the reader the order of evaluation of arithmetic operations / logical operations.  We should add parenthesis to make sure the intention of the code is clear.

#### Why should be used instead
- Parenthesis
