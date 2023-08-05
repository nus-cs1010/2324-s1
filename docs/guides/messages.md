# Common `clang` Errors and Warnings

Here is the list of common `clang` errors and warnings that you may encounter.  We will expand this list over the semester.

## How to Read the Messages

`clang` messages always start with the name of the file, the line number, and the character position. For instance,

```
error.c:3:5: error: use of undeclared identifier 'x'
```

shows the error in the file `error.c`, line 3, position 5.  

!!! tips "" 
    In `vim`, you can use `:<line number>` to jump directly to the line containing this error.



## Variables
### Error: Use of undeclared identifier

C is a static type language, and thus every variable used must be declared with its type.  
E.g.,

```C
int main()
{
    x = 0;
}
```

would lead to the error
```
x.c:3:5: error: use of undeclared identifier 'x'
    x = 0;
    ^
```

In the example above, `x` is used by not declared.  To fix, declare `x` with its type.
E.g.,
```C
int main()
{
    long x = 0;
}
```

### Error: Redefinition of a variable

Each variable should be declared exactly once within its scope (scoped by `{` and `}`).

E.g.,

```C
int main()
{
    long x = 0;
    long x = 1;
}
```

Would give the error
```
x.c:4:10: error: redefinition of 'x'
    long x = 1;
         ^
x.c:3:10: note: previous definition is here
    long x = 0;
         ^
```

To fix, check whether you intend the second declaration to be the same variable (in which case, remove the declaration) or a new one (in which case, give it a different name).

### Warning: Unused Variable

Declaring variables that are not used clutters the code.  It is a good programming practice to only declare the variables that you need.  CS1010 insists on this.  If you declare variables that you end up not using, you will be penalized.

```C
int main()
{
    long x = 1;
}
```
Would result in
```
x.c:4:10: warning: unused variable 'x' [-Wunused-variable]
    long x = 1;
         ^
```

To fix, go through all such warnings and remove any variables that you declared/initialized but never used.

### Warning: Variable May Be Uninitialized

A variable is uninitialized if it is declared but not assigned any value.  This might lead to bugs in your code.

```C
int main()
{
	long y;
	return y;
}
```
Would result in
```
x.c:4:10: warning: variable 'y' is uninitialized when used here [-Wuninitialized]
  return y;
         ^
x.c:3:9: note: initialize the variable 'y' to silence this warning
  long y;
        ^
         = 0
```

To fix, initialize the variable to the appropriate value.

### Warning: Declarations shadows a local variable.

Avoid naming a variable the same name as another variable in the outer scope. Doing so makes your code confusing to read.
E.g.,

```C
int main() {
  long x = 0;
  if (x < 0) {
    long x = 1;
  }
}
```
causes the following warning:
```
x.c:4:10: warning: declaration shadows a local variable [-Wshadow]
    long x = 1;
         ^
x.c:2:8: note: previous declaration is here
  long x = 0;
       ^
```

### Warning: No previous extern declaration for non-static variable

A global variable is detected.  The use of global variables is bug-prone and should be avoided.
For instance,

```C
int x;
int main() {
  x = 1;
}
```
would lead to
```
x.c:1:6: warning: no previous extern declaration for non-static variable 'x'
      [-Wmissing-variable-declarations]
long x;
     ^
x.c:1:1: note: declare 'static' if the variable is not intended to be used outside of this translation
      unit
long x;
^
```

To fix, make the variable local and pass it around from function to function.

## Functions

### Warning: Type specifier missing

Functions must have a return type declared.  C, by default, treats all functions as returning `int` if the return type is not declared.  It is, however, a good programming practice to always declare the return type explicitly, even if it is returning `int`.  CS1010 insists on this, and you will be penalized if you do not declare the return type.

E.g.,

```C
main() 
{
}
```
would give the warning
```
x.c:1:1: warning: type specifier missing, defaults to 'int' [-Wimplicit-int]
main()
^
```

### Warning: Implicit declaration of function

All functions in C must be declared before they are used.  If the function is defined elsewhere, the header file containing the function declaration should be included.  Without the function declaration, the compiler will guess the type of the arguments and their return type.  An incorrect guess would lead to buggy code and thus should be avoided.
E.g.,

```C
main() 
{
	sqrt(4);
}
```
would give the warning
```
x.c:3:4: warning: implicitly declaring library function 'sqrt' with type 'double (double)'
      [-Wimplicit-function-declaration]
   sqrt(4);
   ^
```

To fix, include the appropriate header file.

### Error: Undefined reference to a function

This error is usually accompanied by an "implicit declaration of function" warning.  During linking, `clang` tries to locate the definition of a function.  Calling a function that is not defined would lead to the error above. E.g.,

```C
int main() {
	foo();
}
```
would give the error:
```
/tmp/x-edd854.o: In function `main':
x.c:(.text+0xb): undefined reference to `foo'
clang: error: linker command failed with exit code 1 (use -v to see invocation)u
```

### Error: Too many/few arguments to a function call

Each function should be called with exactly the number of arguments defined.

E.g.,
```C
#include <math.h>
int main() 
{
	sqrt();
}
```
would lead to:
```
x.c:4:10: error: too few arguments to function call, single argument '__x' was not specified
    sqrt();
    ~~~~ ^
```

To fix, check the documentation or the `man` page of the function you are calling to understand the number of arguments needed.

### Warning: Control reaches the end of non-void function

Every non-void function, except `main`, must return a value.  If you define a non-void function but did not include a `return` statement, the compiler would warn you.  Failing the return the intended value means the caller would not receive the correct value back, leading to a buggy code.

E.g.,

```C
int foo() 
{
}
```
would lead to the warning:
```
x.c:3:1: warning: control reaches end of non-void function [-Wreturn-type]
}
```

To fix this, double-check if the function needs to return anything.  If not, change the return type to `void`.  Otherwise, return the appropriate value.

### Warning: Parameter is not declared, defaulting to type `int`

The type of each parameter to a function must be declared explicitly.  Not doing so would lead to code that is cognitively harder to understand and bug-prone than necessary.

For example,

```C
int foo(x) {
  return x;
}
```
would give
```
x.c:1:9: warning: parameter 'x' was not declared, defaulting to type 'int' [-Wpedantic]
int foo(x) {
        ^
```

To fix, declare an appropriate type for each parameter.

### Warning: Unused Parameter

Every parameter that you pass into a function must serve a purpose and so should be used.

For instance,

```C
int foo(long x) {
	return 0;
}
```
leads to the warning:
```
x.c:1:14: warning: unused parameter 'x' [-Wunused-parameter]
int foo(long x) {
             ^
```
To fix, either remove the parameter `x` if you do not need it or check that you do not unintentionally leave `x` unused.

## Logic

### Warning: Expression result unused

The result of your expression should be used.  Otherwise, the computation is wasted.
For instance:

```C
int main() {
  long x = 0;
  x + 2;
}
```
causes the warning:
```
x.c:3:5: warning: expression result unused [-Wunused-value]
  x + 2;
  ~ ^ ~
```

To fix, check if the expression is necessary.  If so, use it as intended. Otherwise, remove it.

### Warning: Code/return/break will never be executed

The execution flow of your code is incorrect.  Part of the code will never be executed and is redundant.

```C
int main() {
  long x;
  return 0;
  x = 1;
}
```
causes the warning:
```
x.c:4:7: warning: code will never be executed [-Wunreachable-code]
  x = 1;
      ^
```

To fix, check the logic of your code and remove redundant code.

### Warning: Comparing floating point with == or != is unsafe

Floating numbers should never be compared with `==` operator since the representation is not precise.

```C
void foo(double x) {
  if (x == 0.03) {
  }
}
```
causes the warning
```
x.c:2:9: warning: comparing floating point with == or != is unsafe [-Wfloat-equal]
  if (x == 0.03) {
      ~ ^  ~~~~
```

To fix this, use `>` and `<` comparison with a small error.  For instance,
```C
if (x > 0.003 - EPSILON && x < 0.003 + EPSILON) {
}
```
where EPSILON is a very small number.

