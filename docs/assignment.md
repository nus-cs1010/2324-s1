# Programming Assignments and Exercises

Programming cannot be "studied".  Students must practice what they learn, through solving programming problems, to attain the skills of computational problem-solving in C.

CS1010 provides close to 90 programming questions for students to practice.  Among these, about 25 of the questions are graded.  These are organized into 8 programming _assignments_.  The remaining questions are ungraded and are organized into 14 programming _exercises_. 

## General Advice

- Spend some time thinking about the algorithm before you begin coding.  Write out the pseudo-code or draw out the flowchart on your own before you start typing in your program. 

- Practice incremental coding.  Incremental coding means do NOT type in the whole long program in a single session and then compile it. Instead, type your program in bits and pieces and compile it incrementally. Try to maintain a compilable program even while you are working on it.  Submitting a compilable program that partially works is better than submitting an un-compilable one; this is especially important for your practical exams. 

- Decompose a problem into smaller or simpler solvable problems.  When a problem is too complex, break the problem down into smaller sub-problems and solve each one separately (by writing functions), before combining them into a solution that solves the original problem.  This way, you can compartmentalize your thoughts and focus on solving smaller, simpler, more tractable sub-problems. 

- If you can't solve a given problem fully, sometimes it helps to make additional assumptions to simplify the problem first (e.g., solve it for the case where the input integer is positive only), before solving the original problem.  During the practical exams, this strategy could help you get partial marks.

- Test your programs thoroughly.  We might not give you all the test cases in the programming questions.  So seeing a `passed` message (meaning your code passed all the given test cases) does not necessarily mean your code is correct.  You should separately run and test each of your programs with additional test cases.

- Follow the problem specification exactly.  Read the problem statement carefully and make sure your code meets the requirement given.  For instance, if the question asks for a function named `compute_area`, then your code MUST contain a function named `compute_area`.  Any other names (`calculate_area`, `ComputeArea`) are not acceptable.  If the output requires a single integer to be printed, printing extra text (e.g., `The answer is 5`) is not acceptable.

- You are encouraged to discuss the programming questions with your friends and learn from each other.  This process is an important part of the learning experience in CS1010.  Sharing and learning about _how to solve a problem_ (at the algorithm level) from each other is not plagiarism.  Copy-pasting of code from others, or _writing the code_ together line-by-line, however, is plagiarism.

## Retrieving and Submitting Programming Questions

All instructions below are meant to be run on the [PE hosts](environments.md).

Every programming assignment and exercise has a unique ID.  An ID is prefixed with either `as` (for an assignment) and `ex` (for an exercise) and is followed by a two-digit sequence number. 

We use [GitHub Classroom](https://classroom.github.com/classrooms/110090574-cs1010-22-23-s1) for managing the submission (including submissions history) and grading.  You will have one code _repository_ for each of the assignments and exercises.

The steps for completing a programming assignment/exercise are as follows:

1. __Accept__: Upon release, log into your GitHub account registered with CS1010 and click on the given Web link to accept the assignment/exercise.  This step would cause a repository to be created for you on GitHub Classroom, and a copy of the skeleton code and test data to be cloned inside that repository.  The name of the repository is of the form `<id>-<username>`.  For instance, `as01-ooiwt`.

2. __Get__: On one of the PE hosts, run:
```
~cs1010/get <id>
```

    For example, to get the 1st assignment, run `~cs1010/get as01`.  This step would cause a copy of your repository to be cloned into the current directory on the PE hosts.  You now have a copy of the code and test data on the PE hosts.

3. __Solve__: Read the questions posted online and solve each question.  More details, along with some best practices for solving the programming questions, are given below.  Note that while there are ways for you to work on GitHub environment (using GitHub CodeSpace or VS Code) directly without logging into PE nodes, doing so is not recommended.  During the practical exams, you are required to solve the programming questions in a sandboxed environment.  The PE hosts emulate the practical exam condition closely.  So it is important for you to become comfortable using the Unix CLI and Vim of the PE hosts.

4. __Submit__: When you want to take a snapshot of your code or submit the final version for grading, run:
```
~cs1010/submit <id>
```

    This process uploads your C code for each question onto GitHub.  Note that any additional files in the directory will not be uploaded.

    For example, `~cs1010/submit as01`.  You should see a message like this:
```
You have submitted your code.  Please verify your submission online at:
  https://github.com/nus-cs1010-2223-s1/as01-ooiwt
to make sure that everything is in order.
```

    After submission, it is a good practice to double-check that your submission is done properly by going to the GitHub site.

    Note that from Assignment 3 onwards, justifications that fall under the category of "failure to submit properly" cannot be accepted as reasons to waive the late submission penalty.

    You are not allowed to interact with your CS1010 GitHub repositories using `git` commands or edit your files directly on GitHub's website.  Doing so would interfere with the automation that we use for grading and would result in penalties (for graded assignments).

## Code Skeleton

After running `~cs1010/get <id>`, you should see the folder `<id>-<username>` in your current directory with skeleton code inside.

Inside that directory, you should see the following files:

- Files that end with `.c`, one for each problem/question.  The naming convention is `<problem>.c`.  These are the skeleton C code that you should edit to solve the programming question and the only files that you should change.
- `inputs` and `outputs` are subdirectories that contain test inputs and test outputs. We use the convention `<problem>.<id>.in` for input test data, and `<problem>.<id>.out` for output test data.  For example, you will see files such as `echo.1.in`, `divide.1.out`, etc.  The expected output for `echo.1.in` is in `echo.1.out`.  You can look at the content of these files if you wish (which [UNIX](unix.md) command should you use to do this?).  You can edit these files to change the test input and  
- `Makefile`: The configuration for the tool `make` that we use to automate the compilation and testing of the programs.  
- `test.sh`: A bash script for testing your code.
- `compiler_flags.txt` and `.clang-tidy` are two files used to configure `clang` and `clang-tidy` respectively.  You do not need to edit this.

## Automating the Edit-Compile-Run Cycle

`make` is a programmer's utility to automate the workflow of the edit-compile-run cycle.  We use `make` for all your exercises, assignments, and practical exams.

A `Makefile` is provided for each of your assignments, exercises, and practical exams.  You don't have to know how to write a `Makefile`, but interested students can contact the teaching team for learning resources.

For most situations, you only need to run:
```
make
```

Our `Makefile` is configured so that `make` performs the following three tasks, in order:

1. __Compile__.  For each `<problem.c>`, `make` invokes the compiler `clang` with the proper arguments to compile the code into binary executable `<problem>`.  This step is executed only if `<problem>.c` has been modified since the last compilation.  If there is a compilation error, `make` would not continue with the rest of the process.

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

Sometimes it is useful to test run a particular program only.  For example, if you want to test only the program `echo`, then
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

## Programming Assignments
### Timeline

The programming assignments are released every Thursday morning.  It is usually due the following Tuesday at 11:59 PM.  

Unless otherwise specified, the grading of each assignment is expected to be completed before the respective Thursday lab session.  Students should receive comments on their code through GitHub before they embark on the next assignment.

The marks, however, may still be changed after that.  wei Tsang may moderate the grading across different groups.  

### Late Submission

All programming assignments must be submitted on time.  If you need an extension, please ask for one and provide a justification for approval.  Only academic reasons and compassionate reasons can be considered (e.g., representing NUS for a sports event is OK; Attending a wedding is not.)

For late submission, there is a 1% penalty (of the total awarded marks for that particular assignment) for every 5-minute after the deadline, capped at 80%.  For example, if an assignment is awarded 40 marks, and is submitted 100 minutes after the deadline, the student will get 32 marks instead (20% penalty).  If it is submitted 10 hours after the deadline, the student will get 8 marks (as it has hit the cap of 80% penalty).

### Method of Submission

Students must use the `submit` command provided on the PE hosts to submit the assignments (more details below).  Programs submitted through other means, such as emails, will NOT be accepted.

### Grading

Only the latest submission of each assignment will be graded.  

Marks are given for attempt, correctness, design, style, and documentation.  The weight of each component will be adjusted over the semester.  

A program that cannot compile (i.e., there is a compilation error) will receive 0 marks for correctness.

In addition, there is an additional -1 mark for each warning that your program received.  Always make sure that your program compiles cleanly without any warning.

For each assignment, we will provide you with a limited set of test data.  During grading, we may grade your program with additional test data.

Grading will be done by both a bot and a human (i.e., a lab tutor).

Feedback will be provided by the tutors as comments in your assignment's GitHub repository.  Any deductions by the lab tutors will be reflected in GitHub.  

A summary of grading "GradingReport.md" will be made available in your assignment's GitHub repository.  This summary reflects the final score, which may include additional deductions by the bot.

### Disallowed Syntax

Some programming assignments may explicitly disallow the use of certain syntax.  Generally, using syntax or statements which are not yet covered either in class or the assignment statement is strongly discouraged.  We also discourage or ban the use of certain syntax for this module, (e.g., `++`) you should not use them.  The assignments are designed such that you should not need to do so (even though doing so may result in your program being shorter or more efficient). If the objective of the assignment is undermined, the penalty for using such forbidden syntax will be heavy. If in doubt, please ask for clarification.

You can find the list of banned and discouraged C features in the article "[C in CS1010](c-in-cs1010.md)."

### Plagiarism

You are NOT to copy from others (including your seniors) or allow others to copy your programs.  We take plagiarism seriously.  See [our policies](policies.md) page for details.

This means that you should also guard your solution carefully, not posting them to publicly accessible places, or changing the permissions of the files on the CS1010 PE hosts so that it is accessible by others.

We routinely run similarity checks on a submitted program, against the bank of submissions from students (including from previous years).  

Copying others' programs will only offer a short-term reprieve and minimal gain in marks for programming assignments.  You would lose the insights, experience, and confidence in solving programming problems, and these losses could lead to larger, longer-term, negative impacts during the CS1010 practical exams, subsequent modules, job interviews, or even your careers.

### Use of Piazza
If you have doubts about the problem statements of a question, you may raise them on Piazza.  But before that, please read through the problem statements carefully first, and check if the same questions have been asked and answered on the forum.

Please exercise discretion when posting to Piazza.  Do not spoil the fun of the "eureka" moments of figuring things out by posting the solution.  

Before the deadline, you are NOT to post the solution to the assignment, complete or partial, on Piazza (or any publicly accessible online site).

## Ungraded Exercises

The ungraded exercises are released on a more ad-hoc basis.   

It is useful to submit your attempts even if it is not graded.  Your attempts will be archived on GitHub and can be shared with your tutor for discussions and feedback if needed.

---
This guideline is expanded from Aaron Tan's CS1010 guideline.
