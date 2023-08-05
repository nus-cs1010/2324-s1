# CS1010 Compilation Guide

We have automated the steps to compile C programs for all CS1010 assignments and exercises by providing `Makefile` and `compile_flags.txt`.  The following guide explains how one can compile a C program with `clang` directly, without using `Makefile`.  This information is useful once students graduated from CS1010 and go to other modules/scenarios where `Makefile` is not provided.

## 1. Compile a standalone C program

Suppose we have a standalone C program `teh.c` that does not use any external libraries.  We can compile the program using the command

```
ooiwt@pe118:~$ clang teh.c
```

This command should create an executable called `a.out` in the current directory, which you can then run with:

```
ooiwt@pe118:~$ ./a.out
```

## 2. Renaming executable file

The name `a.out` is an abbreviation for _assembler output_, a name that many compilers kept as the default output name since the 60s.  We should, however, give our executable more descriptive name, by using the `-o` flag.  (`o` is the mnemonic for output).

```
ooiwt@pe118:~$ clang teh.c -o teh
```

or

```
ooiwt@pe118:~$ clang -o teh teh.c
```

The command above would create an executable called `teh`.

!!! warn "Beware of the order"
    If you are not careful and run the following command instead:
	```
	ooiwt@pe118:~$ clang -o teh.c teh
	```
	`clang` would overwrite your code `teh.c` -- all your hard work will be gone!!

## 3. Warning for possible bugs.

The `clang` checks for syntax errors in your C files -- i.e., things that violate the C syntax rules.  The compiler, however, is smart enough to identify possible bugs -- errors that will cause your program to behave incorrectly, even if the syntax follows C's rules.  You can ask `clang` to warn you about this, using the `-W` flag (`W` is the mnemonic for warning -- note the capital W).  The manual for `clang` lists different types of warnings that `clang` can warn you about.  For instance, we can ask `clang` to warn us by enabling `-Wall` warnings.  The command to do so is:

```
ooiwt@pe118:~$ clang -Wall teh.c -o teh
```

For beginners, it is _highly recommended_ that you _always_ compile with at least `-Wall`, `-Wextra`, and the `-Wpedantic` flag.

!!! tips "clang warning flags"
    `-Wall` in `clang` does not catually enable all warnings.
	`-Weverything` enables every warning but it could be overwhelming
	for beginners.   In CS1010 assignments, we will provide a `Makefile`
	so that you can use `make` to automate the compilation process. 
	Appropriate warning flags will be enabled for you.

## 4. Generating additional information for debugging.

In order to use the debugger `lldb` to trace through and debug your program, `clang` needs to generate additional information and store them in the executable file.  We can instruct `clang` to generate them with the flag `-g` (`g` for generate).  

```
ooiwt@pe118:~$ clang -Wall -g teh.c -o teh
```

It is recommended that you always compile with `-g` flags during the development phase.  If you need to measure the performance (e.g., how fast it runs) of your program or when you are releasing the program to the public, you can remove the `-g` flag and compile with the optimization flags (e.g., `-O`) instead.  

## 5. Linking with the standard library.

To link with a standard library, we use the `-l` flag to specify the name of the library to link.  For instance, to link with the C standard math library (abbreviated as `m`), you issue the command:

```
ooiwt@pe118:~$ clang -Wall -g teh.c -o teh -lm
```

## 6. Linking with 3rd party library

By default, `clang` looks for headers and libraries in the systems directories (`/usr/include`, `/usr/lib`, etc) and the current working directory.  

If you use a third-party library, you usually need to tell `clang` where to look for the corresponding headers and libraries.  You can use the `-I` flag and the `-L` flag for these purposes. For instance, if you have a library installed under your home called `citadel`, and the file `citadel.h` can be found under `~/citadel/include` and the file `libcitadel.a` can be found under `~/citadel/lib`, to tell `clang` where to find these files, you can compile with:

```
ooiwt@pe118:~$ clang -Wall -g -I ~/citadel/include -L ~/citadel/lib teh.c -o teh -lm -lcitadel
```

For instance, to link with the [CS1010 I/O library](library.md) on the PE nodes, you can run
```bash
ooiwt@pe118:~$ clang -Wall -g -I ~cs1010/include -L ~cs1010/lib teh.c -lcs1010
```

## 7. The file `compile_flags.txt`

The list of compilation flags can get lengthy.  For CS1010 assignments/exercises, we have included all the necessary flags in a file called `compile_flags.txt`.  We can then pass this file with the `@` prefix to `clang`.  `clang` will read the flags from the file.

```bash
ooiwt@pe118:~$ clang @compile_flags.txt teh.c -lcs1010
```
