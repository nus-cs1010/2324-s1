# Unit 26: Structures

## Learning Objectives

After this unit, students should 

- be aware of `struct` as a compound data type in C
- know how to (i) declare a struct, (ii) assign values to the member of a struct, (iii) pass a struct into a function as value and as reference, (iv) return a struct from a function
- aware of `.` and `->` notations for accessing members of a struct
- know how to use `typedef` to define a new type

## Compound Data Type

We have so far been working with numbers, characters, and strings.  Not all real-world objects can be easily abstracted and represented with numbers and characters.  It is useful to create our _compound data type_ that represents real-world objects.  Each object typically has one or more attributes, which can be of different types: A module has a code, a title, and the number of MCs; A person has a name, height, weight, and age; A phone has a model, price, and brand.

If you look back at the code that we have written, often multiple variables are related to each other and "belong together."  When we pass one as an argument into a function, often we need to pass another.  It is useful to group them into a compound data type as well.  For instance: A 1D array and its length; A 2D array, its width, and its height; A pixel, its row, its column, and its color.

## Structure in C

In C, we can define a compound data type using a structure, through the C keyword `struct`.  The syntax looks like this:

```C
struct matrix {
  double** array;
  long num_of_rows;
  long num_of_columns;
};
```

In the definition above, the structure is given a name, `matrix`.  The structure contains three _members_.  The first is a pointer to a 2D array, the other two are the number of rows and number of columns of that array.

**Note** that we need a semicolon `;` after the definition of a `struct`.

Here are a few more examples:

```C
struct circle {
  double x_of_center;
  double y_of_center;
  double radius;
};
```

```C
struct module {
  char *code;
  char *title;
  long mc;
};
```

Just like a variable, a `struct` has a scope, and it follows the same rule as the scope of a variable (i.e., it is valid within the block it is declared in).  Unlike declaration of a variable, it is common to declare a `struct` in the global scope, i.e., outside of any function, so that it is usable within the whole program.

### Declaring and Initializing a Structure Variable

Let's see an example of how we can declare and initialize a structure variable:

```C
struct module cs1010;
cs1010.code = "CS1010";
cs1010.title = "Programming Methodology";
cs1010.mc = 4;
```

Line 1 above declares a variable called `cs1010`.  Lines 2-4 initialize each member of the structure.  Note that we use `.` to access each member.

An alternative is to use a _compound literal_:

```C
struct module cs1010 = {
  .code = "CS1010",
  .title = "Programming Methodology",
  .mc = 4
};
```

Using compound literal is convenient in certain cases, as uninitialized members are set to 0 (similar to initializers of arrays).

You can read and write to individual members of a structure variable just like any other variable.

```
cs1010.mc = hours_spent_per_week/2.5;
cs1010_println_long(cs1010.mc);
```

### Assigning a Structure Variable

We can assign one structure variable to another.

```C
struct module cs1010s = cs1010;
```

This assignment statement above is equivalent to assigning each member of the `struct` individually.

### Structure as Parameters

We can pass a structure variable into a function just like a non-array variable.  Unlike an array, a `struct` is called by value, i.e., it is copied onto the call stack of the function.

Hence, the code below does not actually update the MCs of CS1010:

```
void update_mc(struct module cs1010, long hours_spent_per_week) {
  cs1010.mc = hours_spent_per_week/2.5;
}
```

To call a structure by reference, we can pass in its pointer.

```
void update_mc(struct module *cs1010, long hours_spent_per_week) {
  (*cs1010).mc = hours_spent_per_week/2.5;
}
```

The latter example is more common idiom.  Since a structure can contain multiple fields and usually occupies more bytes than a pointer, it is less efficient to copy a structure onto the call stack compared to copying its pointer.  

Since this is common, C provides another syntax for accessing the member of a structure through its pointer, using the "arrow" notation"

```
void update_mc(struct module *cs1010, long hours_spent_per_week) {
  cs1010->mc = hours_spent_per_week/2.5;
}
```

### Returning a Structure

A function can return a structure.  Remember in [Unit 16](16-call-by-reference.md) we said that C functions can return only one value and one way to get around this limitation is to use call by reference and the other is to use `struct`?  Here is how we use `struct` to return more than one values:

```
struct answer {
  long max_n;
  long max_num_steps;
};
```

```C
struct answer find_max_steps(long n) {
  struct answer ans = {
    .max_n = 1,
    .max_num_steps = 0
  };
  for (long i = 1; i <= n; i += 1) {
    long num_of_steps = count_num_of_steps(i);
    if (num_of_steps >= ans.max_num_steps) {
      ans.max_n = i;
      ans.max_num_steps = num_of_steps;
    }
  }
  return ans;
}
```

When a function returns a `struct`, the structure gets _copied_ back to the caller.


### Defining a Structure as a Type

To avoid writing the keyword `struct` every time we declare a variable or parameter, let's introduce another keyword in C: `typedef`.

C allows programmers to define their type based on the existing types.  Suppose that I want a type that represents a person in my contract tracing app, and I represent each person with an positive integer id.  I can define:

```C
typedef unsigned long person_t;
```

Recall that we use the suffix `_t` convention to denote user-defined type.  You have probably seen these two types elsewhere `size_t` and `time_t` in the past.

Now that we have defined `person_t` as a new type that is equivalent to `unsigned long`, we can use it just like another type:

```
void is_contact(person_t i, person_t j);
```

Using user-defined type can add more "semantics" to the code and make it easier to understand.

Using `typedef` on `struct` frees us from typing the word `struct` every time.  We can do so with either:

```C
typedef struct module {
  char *code;
  char *title;
  long mc;
} module;
```

or


```C
typedef struct {
  char *code;
  char *title;
  long mc;
} module;
```

In either case, we can just use `module` like any other type:

```
void update_mc(module cs1010, long hours_spent_per_week) {
  cs1010.mc = hours_spent_per_week/2.5;
}
```

If you use third-party libraries or C libraries, chances are you will come across such types.  The use of `typedef` on `struct` is controversial.  There is a school of thought that thinks it makes the code harder to read as it obscured the fact that a variable is a struct.  Hidden cost in copying the variable onto the call stack as a parameter or returned value becomes non-obvious.  Interested students can read [Linux's Kernel Coding Style](https://www.kernel.org/doc/html/v4.10/process/coding-style.html#typedefs) for the pros and cons of this approach.
