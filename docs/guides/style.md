# CS1010 C Style 

In CS1010, you should following the following style guide when you write your code for your graded homework and practical exams.  We may deduct marks for coding style if your code egregiously violates the style.

This guide is modified from past CS1010 style guide by Aaron Tan.

## Why Coding Style is Important

Quote

"It is not merely a matter of aesthetics that programs should be written in a particular style. Rather there is a psychological basis for writing programs in a conventional manner: programmers have strong expectations that other programmers will follow these discourse rules. If the rules are violated, then the utility afforded by the expectations that programmers have built up over time is effectively nullified. The results from the experiments with novice and advanced student programmers and with professional programmers described in this paper provide clear support for these claims."

Elliot Soloway and Kate Ehrlich. "Empirical studies of programming knowledge." IEEE Transactions on Software Engineering 5 (1984): 595-609.

## 1. Variable Declaration

Each variable should be declared in its own line.

```C
double weight;  // The weight of the baby
double height;  // The height of the baby
```

Avoid

```C
double weight, height;   // Weight and height of the baby
```

## 2. Give Variables Descriptive Names

This is the most important rule to follow.  The name of a type, variable, function, constant should inform us of its purpose clearly without the readers having to guess or look up its meaning.

For example, `long number_of_coins;` is an appropriate variable but not `long c;`. Avoid using a single character for variable names.

There are some exceptions, however, as shown below:

- If the variable is the problem size and it is given in the task statement. For example, a problem dealing with n values, hence the variable may be called `n` (preferably with a comment to explain).
- If the variable is a transient/temporary variable whose purpose is clear.
- If the variable is a loop variable whose purpose is clear.

## 3. Shorten Variable Names with Naming Conventions

Despite the recommendation for descriptive names, identifiers can be short yet descriptive by using abbreviations and/or common naming conventions. For example, `MAX_LEN`, `num_of_elems`, `pcurr`, `table_num`.

However, do not invent your own abbreviation. For instance, names like `nm_elemnts` should be avoided.

## 4. Avoid Negated Variable or Function Names

Negated variables often result in hard-to-read double-negatives in an expression like `!is_not_err`.

So, avoid `is_not_error`, `is_not_found`, `is_not_valid`, `cannot_open_file`.  Instead, we prefer `is_error`, `is_found`, `is_valid`, `can_open_file` etc.

## 5. Use `#define` to Define Constants for Magic Numbers

Avoid direct use of magic numbers. Constant literals which have special meanings should be named and its named identifier should be used in its place. For example:

Avoid:
```C
for (i = 0; i < 100; i += 1) {
    :
}
```

Prefer:
```
#define MAX_LEN 100
 :
for (i = 0; i < MAX_LEN; i += 1) {
    :
}
```

## 6. Naming Conventions

### Constants

All constant identifiers must be written in all caps and separated by an underscore `_`.  For instance `MAX_ITERATIONS`, `MAX_LEN`, `GOLDEN_RATIO`, `COLOR_DEFAULT`, `PI`.

### Variables and Functions

Use lower case letters for variable names and function names, with multiple words separated by underscore `_`.  Example, `cs1010_read_long`, `is_prime`.  This convention is known as the _snake case_.

## 7. Use Consistent Indentation to Emphasize Block Structure

The code should be properly and neatly indented to emphasize the nested logical structure of the program. An indentation of 2 or 4 spaces is recommended (8 is too wide).

Every block that follows a `for`, `while`, `if-else`, `do-while` statement must be indented from its enclosing block.

Comments within a block should follow the indentation level of its enclosing block. For example,

```C
for (i = 0; i < 3; i += 1) {   
    // Comments should be indented too
    while (j != i) {
        // More indented comments
        cs1010_println_string("Hello");
    }
}
```

The following are the wrong ways to indent the comments.

```C
for (i = 0; i < 3; i += 1) {
// This comment should be indented and aligned with the while statement.
    while (j != i) {
    // This comment should be aligned with the printf statement.
        cs1010_println_string("Hello");
    }
}
```

## 8. Don't Mix Tabs and Spaces

You must use only spaces in your code.  Do not use tabs.

You can add the configuration `set expandtab` in `vim` to automatically expand any tab that you enter into spaces.

## 9. Spaces in `if`, `else`, `for`, `while`, `do`-`while` Statements

Add a single space between the keywords `if`,`else`, `for`, `while` and the following parentheses and between the parentheses and next curly bracket.  For instance:

```
for( ... ) { // not good
for( ... ){ // not good
for ( ... ) { // good
```

## 10. Spaces in Assignments

Add a single space before and after `=`.

```
a=b; // no
a= b; // no
a =b; // no
a = b; // OK!
```

## 11. Positions of Open and Close Braces

There are two camps on the position of open braces. The following shows the "trailing open braces":

```C
for (i = 0; i < 3; i += 1) {
    while (j != i) {
        printf("Hello\n");
    }
}
```

The following shows the "leading open braces". The leading open brace must be aligned with the block of the construct it is in:

```C
for (i = 0; i < 3; i += 1) 
{
    while (j != i) 
    {
        printf("Hello\n");
    }
}
```

Both styles are acceptable, but you should be consistent and should not mix both styles in a single program.

For close braces, they should be leading close braces aligned with the block of the construct. Close braces should NOT be trailing as that would make it hard to spot them.

# 12. Avoid Else After Return

Adding an `else` after a `return` statement in the `if` block is unnecessary and increases the indentation level.  If the last statement of the `if` block is a `return` statement, we should skip the `else` statement. 

```C
if (x == 1) {
    return 10;
} else {
    if (y == 2) {
        return 13;
    } else {
        return 89;
    }
} 
```

can be written as the following equivalent code:
```C
if (x == 1) {
    return 10;
} 
if (y == 2) {
    return 13;
} 
return 89;
```

## 13. Comment Major Code Segments Adequately

Major segments of code should have explanatory comments. A major segment may be a loop block or a function block.

You should comment on complicated logic, expressions, or algorithms, explaining what you are doing in the code, including why and how.

An "if" block with a complex condition or an expression that is hard to understand should have explanatory comments.  For example,

```C
// Check and reject out-of-bounds indices
if (k < 0 || k >= MAX_LEN) {
    return -1;
}
```

## 14. Avoid Superfluous Comments

A comment such as:

```C
i += 1; // add one to i
```

serves no purpose, adds clutter to a program and does more harm than good.

## 15. Blank Lines

It is good to add a blank line between two functions, or two long segments of code for readability.

```C
// This function ...
int f(int x) {
    // body
}

// This function ...
int g(double y) {
    // body
}
```

```C
// Statements 1 to 10 belong to a sub-task
statement1;
statement2;
   :
statement10;

// Leave a blank line for readability
statement11;
statement12;
   :
```

However, guard against the use of excessive blank lines. Double blank lines and triply blank lines, or more, should not be present.


## 16. Long Lines

If a line (be it a statement or a comment) is too long (more than 80 characters), do not let it run through the screen and wrap around. Instead, split it into a few lines.

```C
if ((has_cs2010 || has_cs2020 || has_cs2040 || has_cs2040C) && (has_st1232 || has_st2131 || has_st2132 || has_st2334) && (has_ma1102R || has_ma1505 || (has_ma1511 && has_ma1512) || has_ma1521) && (has_ma1101R || has_ma1311 || has_ma1506 || has_ma1508E)) 
```
is bad.

```C
if ((has_cs2010 || has_cs2020 || has_cs2040 || has_cs2040c) && 
   (has_st1232 || has_st2131 ||  has_st2132 || has_st2334) && 
   (has_ma1102r || has_ma1505 || (has_ma1511 && has_ma1512) || has_ma1521) &&    
   (has_ma1101r || has_ma1311 || has_ma1506 || has_ma1508e)) 
```
is better.
