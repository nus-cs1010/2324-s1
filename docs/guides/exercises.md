# Programming Exercises

Programming cannot be "studied".  Students must practice what they learn, through solving programming problems, to attain the skills of computational problem-solving in C.

CS1010 provides close to 90 programming questions for students to practice.  These are organized into 11 weekly programming exercises.

## General Advice

- Spend some time thinking about the algorithm before you begin coding.  Write out the pseudo-code or draw out the flowchart on your own before you start typing in your program. 

- Practice incremental coding.  Incremental coding means do NOT type in the whole long program in a single session and then compile it. Instead, type your program in bits and pieces and compile it incrementally. Try to maintain a compilable program even while you are working on it.  Submitting a compilable program that partially works is better than submitting an un-compilable one; this is especially important for your practical exams. 

- Decompose a problem into smaller or simpler solvable problems.  When a problem is too complex, break the problem down into smaller sub-problems and solve each one separately (by writing functions), before combining them into a solution that solves the original problem.  This way, you can compartmentalize your thoughts and focus on solving smaller, simpler, more tractable sub-problems. 

- If you can't solve a given problem fully, sometimes it helps to make additional assumptions to simplify the problem first (e.g., solve it for the case where the input integer is positive only), before solving the original problem.  During the practical exams, this strategy could help you get partial marks.

- Test your programs thoroughly.  We might not give you all the test cases in the programming questions.  So seeing a `passed` message (meaning your code passed all the given test cases) does not necessarily mean your code is correct.  You should separately run and test each of your programs with additional test cases.

- Follow the problem specification exactly.  Read the problem statement carefully and make sure your code meets the requirements given.  For instance, if the question asks for a function named `compute_area`, then your code MUST contain a function named `compute_area`.  Any other names (`calculate_area`, `ComputeArea`) are not acceptable.  If the output requires a single integer to be printed, printing extra text (e.g., `The answer is 5`) is not acceptable.

- You are encouraged to discuss the programming questions with your friends and learn from each other.  This process is an important part of the learning experience in CS1010.  Sharing and learning about _how to solve a problem_ (at the algorithm level) from each other is not plagiarism.  Copy-pasting of code from others, or _writing the code_ together line-by-line, however, is plagiarism.

See also [How NOT to Go About a Programming Assignment](http://people.irisa.fr/Martin.Quinson/Teaching/how-not-to-code.pdf), by Agustin Cernuda del Rio. 

> Computer programming students invariably fall into more than one bad habit. It can be extremely difficult to eradicate them (and many lecturers and professional programmers keep succumbing to them time and again).._

## Retrieving and Submitting Programming Questions

All instructions below are meant to be run on the [PE hosts](environments.md).

Every programming exercise has a unique ID, prefixed with `ex`, and is followed by a two-digit sequence number. 

We use [GitHub Classroom](https://classroom.github.com/classrooms/141557850-cs1010-23-24-s1) for managing the submission (including submissions history) and grading.  You will have one code _repository_ for each exercise.

The steps for completing a programming assignment/exercise are as follows:

1. __Accept__: Upon release, log into your GitHub account registered with CS1010 and click on the given Web link to accept the assignment/exercise.  This step would cause a repository to be created for you on GitHub Classroom, and a copy of the skeleton code and test data to be cloned inside that repository.  The name of the repository is of the form `<id>-<username>`.  For instance, `ex00-ooiwt`.

2. __Get__: On one of the PE hosts, run:
```
~cs1010/get <id>
```

    For example, to get the first exercise, run `~cs1010/get ex00`.  This step would cause a copy of your repository to be cloned into the current directory on the PE hosts.  You now have a copy of the code and test data on the PE hosts.

3. __Solve__: Read the questions posted online and solve each question.  More details, along with some best practices for solving the programming questions, are given below.  Note that while there are ways for you to work on the GitHub environment (using GitHub CodeSpace or VS Code) directly without logging into PE nodes, doing so is not recommended.  During the practical exams, you are required to solve the programming questions in a sandboxed environment.  The PE hosts emulate the practical exam condition closely.  So it is important for you to become comfortable using the Unix CLI and Vim of the PE hosts.

4. __Submit__: When you want to take a snapshot of your code or submit the final version for grading, run:
```
~cs1010/submit <id>
```

    This process uploads your C code for each question onto GitHub.  Note that any additional files in the directory will not be uploaded.

    For example, `~cs1010/submit ex01`.  You should see a message like this:
    ```
    You have submitted your code.  Please verify your submission online at:
      https://github.com/nus-cs1010-2324-s1/ex00-ooiwt
    to make sure that everything is in order.
    ```

    After submission, it is a good practice to double-check that your submission is done properly by going to the GitHub site.

    You are not allowed to interact with your CS1010 GitHub repositories using `git` commands or edit your files directly on GitHub's website.  Doing so would interfere with the automation that we use for grading.

## Code Skeleton

After running `~cs1010/get <id>`, you should see the folder `<id>-<username>` in your current directory with skeleton code inside.

Inside that directory, you should see the following files:

- Files that end with `.c`, one for each problem/question.  The naming convention is `<problem>.c`.  These are the skeleton C code that you should edit to solve the programming question and the only files that you should change.
- `inputs` and `outputs` are subdirectories that contain test inputs and test outputs. We use the convention `<problem>.<id>.in` for input test data, and `<problem>.<id>.out` for output test data.  For example, you will see files such as `echo.1.in`, `divide.1.out`, etc.  The expected output for `echo.1.in` is in `echo.1.out`.  You can look at the content of these files if you wish.  You can also edit these files to change the test input and output to modify the test cases.
- `Makefile`: The configuration for the tool `make` that we use to automate the compilation and testing of the programs.  
- `test.sh`: A bash script for testing your code.
- `compiler_flags.txt` and `.clang-tidy` are two files used to configure `clang` and `clang-tidy` respectively.  You do not need to edit this.
- `.gitignore` contains a list of filenames to be ignored by the submission script.

## Automating the Edit-Compile-Run Cycle

`make` is a programmer's utility to automate the workflow of the edit-compile-run cycle.  We use `make` for all your exercises, assignments, and practical exams.

A `Makefile` is provided for each of your assignments, exercises, and practical exams.  You don't have to know how to write a `Makefile`, but interested students can contact the teaching team for learning resources.

For most situations, you only need to run:
```
make
```

Our `Makefile` is configured so that `make` performs the following three tasks, in order:

1. __Compile__.  For each `<problem.c>`, `make` invokes the compiler `clang` with the proper arguments to compile the code into binary executable `<problem>`.  This step is executed only if `<problem>.c` has been modified since the last compilation.  If there is a compilation error, `make` will not continue with the rest of the process.

2. __Test__.  If the compilation is successful or not needed, `make` would invoke `test.sh <problem>` to test your program against each of the given test cases.  If your program passed all the given test cases, it would print:
```
<problem>: passed
```
Otherwise, a list of failed test cases is printed.

3. __Lint__. If the compilation is successful or not needed, and regardless of whether your code passed all the test cases or not, `make` would invoke `clang-tidy` to check your code against bad programming practices.  This step generates warnings for each instance of bad practices or bug-prone code found.  You should heed the warnings and fix your code, even if your code passed all the test cases.

If you only want to run `clang-tidy` on all the C files, you can run
```
make tidy
```

If you only want to test all the programs, you can run
```
make test
```

To compile an individual program, type
```
make <program>
```

For instance, the command
```
make echo
```

compiles `echo.c` into `echo` without testing or running `clang-tidy` on it.

Sometimes it is useful to test-run a particular program only.  For example, if you want to test only the program `echo`, then
```
./test.sh echo
```

Finally, you can also run
```
make clean
```
to remove all the generated executable files.

## Testing

If your code prints the wrong output for some of the test inputs, familiarity with Unix CLI would be helpful.  Suppose that your code for problem `echo` fails on test case 3.  To see the input of this test case, run:
```
cat inputs/echo.3.in
```

To see the expected output:
```
cat outputs/echo.3.out
```

To see what output your program gives,
```
./echo < inputs/echo.3.in
```

Sometimes it is useful to store the output of your program in a file:
```
./echo < inputs/echo.3.in > myoutput
```

You can then run `vim -d` to compare the expected output and your output to figure out what exactly differs between the two outputs:
```
vim -d outputs/echo.3.out myoutput
```

For each question, we will provide you with a limited set of test data.  During grading, we may grade your program with additional test data.

### Ignoring Changes in Spaces and Blank Lines

Note that internally, in `test.sh`, we use `diff -bB` to compare if your output matches the expected output.  This means that we ignore blank lines and changes in white spaces.

We require an exact match in the outputs only in programming questions where white spaces matter.

### Correctness of Test Data

You may assume that all input data are correct unless otherwise stated. Hence you do NOT need to do input data validation. This is to allow you to focus on getting the program right, instead of worrying about making your program fool-proof which involves a lot more work. 

## Identifying Yourself

In every C file that you submit to CS1010, it is a good practice to identify yourself by writing your name and lab group.  We may deduct marks if you fail to do so.  You need to edit the line:

```
@author XXXX (Group YYYY)
```

and change it to something like:

```
@author Elsa of Arendelle (Group B10)
```

This helps us to easily identify the authors when needed.  Furthermore, signing off your work is a sign that you take pride in the work that you have produced!

## Submitting and Receiving Feedback

### Method of Submission

Students must use the `submit` command provided on the PE hosts to submit the assignments (more details below).  Programs submitted through other means, such as emails, will NOT be accepted.

### Feedback

Only the latest submission of each question will be reviewed by the tutors after the deadline.

The dimension for review includes correctness, design, style, efficiency, and documentation.  Each submission will be categorized as one of the following four categories:

- Excellent
- Good
- Need improvement
- Insufficient effort 

A program that cannot compile (i.e., there is a compilation error) will be considered as "insufficient effort".

Feedback is done by both a bot and a human (i.e., a lab tutor) and is posted as comments in your GitHub repository. A summary of "Feedback.md" will be made available in your GitHub repository when the review is completed.

Note that, during the practical exam, a program that cannot compile (i.e., there is a compilation error) will receive 0 marks. In addition, there is an additional -1 mark for each warning that your program received.  Always make sure that your program compiles cleanly without any warning when solving the exercises.

### Disallowed Syntax

Some programming assignments may explicitly disallow the use of certain syntax.  Generally, using syntax or statements which are not yet covered either in class or the exercise question is strongly discouraged.  We also discourage or ban the use of certain syntax for this module, (e.g., `++`) you should not use them.  The exercises are designed such that you should not need to do so (even though doing so may result in your program being shorter or more efficient). 

During the practical exam, if the objective of the assessment question is undermined, the penalty for using such forbidden syntax will be heavy. If in doubt, please ask for clarification.

You can find the list of banned and discouraged C features in the article "[C in CS1010](c-in-cs1010.md)."

## Reminders About Course Policies

Please review the following policies related to programming exercises

- [Asking questions and getting help](../about.md#asking-questions-and-getting-help)
- [Use of AI tools](../about.md#use-of-ai-tools)
- [Discussion and plagiarism](../about.md#discussion-and-plagiarism)
- [Late/miss submission policy](../about.md#latemissed-submission-policy)

---
This guideline is expanded from Aaron Tan's CS1010 guideline.
