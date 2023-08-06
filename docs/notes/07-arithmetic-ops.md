# Unit 7: Arithmetic Operations

## Learning Objectives

After this unit, students should:

- be able to define arithmetic expressions in C programs that include the use of the following arithmetic operators:
    - addition ( `+` ),
    - subtraction ( `-` ),
    - multiplication ( `*` ),
    - division ( `/` ), and
    - remainder ( `%` );
- be aware of the numeric types that may be used with arithmetic operators in C programs;
- be aware of the value range restrictions on the various numeric types in C;
- be able to use compound operators in C programs;
- be aware of the difference between division and integer division, and when the latter occurs within arithmetic expressions; and
- be aware that there exist pitfalls in using `++` and `--` operators, and they are not allowed in CS1010.

## Operators

You have seen the `+` operator in the previous units.  You can use `+` to add two variables, a value, and a variable, or two values:

```C
long a = 1;
long b = 2;
long c = 3;
a = b + c; // add two variables
b = a + 4; // add a variable to a value
c = 5 + 6; // add two values
```

You can also use `+` on values returned by functions:

```C
long sum_of_square = square(base) + square(height);
```

You have also seen the multiplication operator `*`.  It can be used in the same way as the `+` operator.  Three other useful operators are:

- `/` - division (e.g., `#!C double half_x = x / 2;`)
- `-` - subtraction (e.g., `#!C long deducted = income - 100;`)
- `%` - remainder (e.g, `#!c long last_digit = number % 10;`)

The `+`, `-`, `*`, and `/` operators work on both integer types (`char`, `short`, `int`, `long`, `long long`) and real numbers (`float`, `double`).  The module operator `%` works only on integer types.

## Operator Precedence

We can chain the operations together to form expressions such as:

```C
long b = 10;
long c = 2;
long a = b + 2 * c / 4;
```

When we have multiple operations appearing, however, it becomes harder to trace the sequence of evaluation.  What is the value of `a` after the three lines above are executed?  There is some ambiguity: do we evaluate from left to right (in which case the answer is 6) or do we multiply and device first before adding (in which case the answer is 11)? 

C has well-defined rules for the order of evaluation for the operators: `*`, `/`, and `%` take precedence over `+` and `-`, and the operators are evaluated from left to right.

Thus, in the example above, `a` will be 11 instead of 6 after the execution.

To change the order of execution, we can add parenthesis to the expression.  For instance:

```C
long a = (b + 2) * c / 4; // 6
long a = b + (2 * c / 4); // 11
```

The expression in the parenthesis will be evaluated first.  To make your code easier to understand, _you should add parenthesis even if the order of evaluation is according to the order of precedence_ to make the order of evaluation explicit[^1].

[^1]: Interested students may refer to [the Wikipedia article on C operators](https://en.wikipedia.org/wiki/Operators_in_C_and_C%2B%2B#Operator_precedence) on the complete rules for precedence.  But for beginners like us, using parentheses to explicitly indicate the order in which we wish to evaluate our computation suffices.

## Compound Operators

It is common to modify the value of a variable and store a new value back in the same variable.  For example,

```C
index = index + 1;  // increment the variable index
age = age * 2;  // double the variable age
```

C provides _compound operators_ that simplify the expressions above.  For example,

```C
index += 1;
age *= 2;
```

The syntax for a compound operator is `op=`, where `op` can be `+`, `-`, `*`, `/`, `%`, or other binary operators.  The statement:

```
a op= b;
```

modifies `a` the same way as:

```
a = a op b
```

## Common Mistakes Using Arithmetic Operations

It is important to remember that when arithmetic operations in C are performed on a sequence of bits, the value that the sequence of bits can represent is limited and is determined by its type.  A common mistake for beginner programmers is to forget this fact and treat the arithmetic operations as the same as the ones seen in mathematics.

Let's look at two common gotchas.

### Overflow

Consider the type `uint8_t`, which represents an unsigned 8-bit integer, and the following code:

```C
uint8_t c = 255;
c += 1;
```

What is the value of variable `c` after the operation above?

Here, we are adding one to the value 255, so `c` must store the value 256, right?

It turns out that after the execution above, `c` contains the value 0.  The variable `c` is of the type `uint8_t`, which is the unsigned 8-bit integer.  Being 8-bit, the variable can store values from 0 to 255.  When we add 1 to 255, even though we get the result 256, mathematically, we cannot store 256 in `c` -- there are not enough bits!  In this case, the value stored is "wrap around", and we get the value 0 instead.

The variable `c` above is unsigned. It gets trickier if `c` is signed.  In the case of overflowing signed integer, the behavior depends on the compiler and is undefined in the C standard.

### Integer Division

Now, let's consider the following code:

```C
double half = 3/2;
```

What is the value of the variable `half` after the operation above?

It got to be 1.5, right?

It turns out that, after executing the code above, the value of `half` is 1.0.  :open_mouth:

To understand this, first, let's see what happens when we assign a floating-point number to an integer type:

```C
int x = 1.5;
```

C truncates the floating number and only stores the integer part of the value, 1 in this case, in `x`.

Second, when we perform an arithmetic operation, the resulting value will be an integer if both values are integer types.  If one of the operands is a floating-point number, the result will be a floating-point number[^2].

[^2]: The actual rules used by C, called _integer promotion_ and _usual arithmetic conversion_, are much more complex and are outside the scope of CS1010.  You should take note of this, however, and in a later part of your study or career, if you need to delve deeper into writing or debugging C code, take a look at [this](https://wiki.sei.cmu.edu/confluence/display/c/INT02-C.+Understand+integer+conversion+rules).

Since 3 and 2 are both integers, the resulting value 1.5 is stored in an integer, which causes it to become 1.  We then store 1 into a `double` variable, causing the value of `half` to become `1.0`.

Because of this limitation, the operation `/` is sometimes also known as integer division when both operands are integers.

In order to get the result 1.5 as expected, we can write either:
```C
double half = 3/2.0;
```

or

```C
double half = (double)3/(double)2;
```

The second fix above explicitly converts the type, or casts the type, of values 2 and 3 into a `double`.

As it is a good practice to be explicit when programming so that the readers of your code knows your intention, in CS1010, we will use explicit casting to convert `long` to `double` when necessary.

```C
long x = 3;
long y = 2
  :
double x_over_y = x / y;  // trigger a compiler warning
double x_over_y = (double)x / (double)y;  // ok
```

## The % Operator

It is commonly misunderstood that the `%` operator is equivalent to the modulo operation in number theory, where the operator always gives a positive remainder.

This equivalence holds when the two operands are positive. E.g., `#!C 9 % 4` gives `#!C 1` as the answer.  But, when one of the operands is negative, the `%` operator may return a negative number.

The `%` operator in C is defined as follows: `#!C x % n` is equivalent to `#!C x - ((x / n) * n)` (where `#!C /` is the integer division operator).

For instance, `#!C -9 % 4` is evaluated as `#!C -9 - ((-9 / 4) * 4)` which is `#!C -9 - (-8)`, and gives `#!C  -1`.

On the other hand, `#!C 9 % -4` is evaluated as `#!C 9 - ((9 / -4) * -4)`, which gives `#!C  9 - 8`, or `#!C  1`.

To avoid confusion between the `%` operator in C and the modulo operation in number theory, we will call the `%` as the _remainder_ operator. 

## Avoid Increment / Decrement Operator

If you read C code in other places, you will certainly come across the increment or decrement operator, `++` or `--`.  The operators add one and minus one from the operand respectively.  So, the statement

```C
index += 1;
```

can be further shortened into

```C
index++;
```

and the statement

```C
index -= 1;
```

can be further shortened into

```C
index--;
```

Using these two operators only shortens your code by two characters per statement, but introduces several issues.  As such, we _ban the use of both increment and decrement operators in CS1010_.

So, why aren't `++` and `--` welcomed in CS1010?  The `++` and `--` operators not only modify the value of the operand, but it also returns a value.  We can write `j = i++;` to both increment `i` and assign the pre-incremented value of `i` to `j`.  In C, we can also write `j = ++i;`, which again, increment `i`, and assign the post-incremented value of `i` to `j`.  Things get tricky when we write `i = i++;`, it is not clear how to interpret this.  The C standard leaves this behavior undefined and leaves it to the compiler to define its behavior.  Introducing all these complexities just to save two characters is not warranted.

## List of C features banned in CS1010

You should realize by now that we are only using a subset of C and enforce a certain style of programming in CS1010.  We wish to move all of you away from common pitfalls in learning and writing C so that we can focus on using C as a tool to solve problems.

So far, you have seen that we are banning:

- `++` and `--` operators
- the types `int`, `short`, `float`, etc. (with the returning type of `main()` as the exception)
- global variables

In addition, we discourage:

- the use of `printf` and `scanf`
- skipping parenthesis in writing arithmetic expression

The [complete list of C features that we ban or discourage in CS1010 can be found here](../guides/c-in-cs1010.md).
