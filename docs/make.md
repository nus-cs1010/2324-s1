# Compiling and Testing with `make`

`make` is a programmer's utility to automate the workflow of the edit-compile-run cycle.  We use `make` for all your exercises, assignments, and practical exams.

A `Makefile` is provided for each of your assignments, exercises, and during practical exams.  You don't have to know how to write a `Makefile`, but interested students can contact the teaching team for learning resources.

For most of the situations, you only need to run:
```
make
```

Our `Makefile` is configured so that `make` performs the following three tasks, in order:
- compile all the `*.c` files, with the correct flags and libraries, to generate the executable binaries.
- run `test.sh` on each of the programs, on each of the inputs, and cross-check if the output is correct.
- run `clang-tidy` on all the `*.c` files to check if you follow good programming habits in your code

`make` is smart enough that, if a C file has not changed since the last compilation, it will not recompile the file.

If you only want to run `clang-tidy` on all the C files, you can run
```
make tidy
```

If you only want to test all the programs, you can run
```
make test
```

Sometimes it is useful to test run a particular program only.  For example, if you want to test only the program `digits`, then

```
./test.sh digits
```

Finally, you can also run
```
make clean
```
to remove all the generated executable files.
