# NUS CS1010 Handbook for AY23/24 Semester 1

#

#### :fontawesome-solid-circle-info: &nbsp;Important [General Information](about.md) About CS1010
#### :fontawesome-solid-people-group: &nbsp;[Teaching Team](team.md)
#### :fontawesome-solid-calendar-days: &nbsp;[Class Schedule](schedule.md)

<br>

=== ":fontawesome-solid-book: Notes"

    # 

    ## Preliminaries

    <div class="grid cards" markdown>

    - __1. Programming__

        What is a program?  What is programming?  What is CS1010 about?
        [:octicons-arrow-right-24:](notes/01-program.md)

    - __2. Computational Problems__
        
        What is a computational problem?  Express a step-by-step solution to a computational problem with a flowchart. 
        [:octicons-arrow-right-24:](notes/02-algo.md)

    - __3. Functions__

        Write functions that solve simpler sub-problems and then combine the functions to solve higher-level and more complex problems.
        [:octicons-arrow-right-24:](notes/03-func.md)

    - __4. Types__

        Each variable has a type that tell the machine how to turn a bit sequence into a meaningful value.
        [:octicons-arrow-right-24:](notes/04-type.md)

    </div>


    ## Basic C
    <div class="grid cards" markdown>

    - __5. First C Program__

        Introducing basic C syntax and writing our first C program.
        [:octicons-arrow-right-24:](notes/05-first-c.md)

    - __6. CS1010 I/O__

        Reading from the stdin and printing to the stdout using the CS1010 library.
        [:octicons-arrow-right-24:](notes/06-cs1010-io.md)

    - __7. Arithmetic Operations__

        Performing basic computation with arithmetic operations with C.
        [:octicons-arrow-right-24:](notes/07-arithmetic-ops.md)

    </div>

    ## Control Statements
    <div class="grid cards" markdown>

    - __8. Conditional Statements__

        Implementing conditional control in C
        [:octicons-arrow-right-24:](notes/08-if-else.md)

    - __9. Logical Expressions__

        How to express compound logical expressions in C
        [:octicons-arrow-right-24:](notes/09-logical-exp.md)

    - __10. Assertions__

        Asserting what must be true at each point of the program.
        [:octicons-arrow-right-24:](notes/10-assert.md)

    - __11. Loops__

        Implementing iterative control in C
        [:octicons-arrow-right-24:](notes/11-loop.md)

    - __12. Invariant__

        Reasoning about loops with invariant
        [:octicons-arrow-right-24:](notes/12-invariant.md)

    </div>



    ## Arrays, Pointers, Memory Management
    <div class="grid cards" markdown>

    - __13. Call Stack__ 

        How calling a function and passing parameters by-value works.
        [:octicons-arrow-right-24:](notes/13-call-stack.md)

    - __14. Fixed-Length Array__

        How arrays are implemented and can be used in C
        [:octicons-arrow-right-24:](notes/14-array.md)

    - __15. Pointers__

        How to access a variable via its address
        [:octicons-arrow-right-24:](notes/15-pointers.md)

    - __16. Call-by-Reference__

        How to modify a variable by passing it into a function
        [:octicons-arrow-right-24:](notes/16-call-by-reference.md)

    - __17. Heap__

        Dynamically allocating and deallocating memory
        [:octicons-arrow-right-24:](notes/17-heap.md)

    - __18. Characters and Strings__

        Representing a string as an array of `char`s
        [:octicons-arrow-right-24:](notes/18-string.md)

    - __19. Multidimensional Array__ 

        Using array of arrays
        [:octicons-arrow-right-24:](notes/19-md-array.md)

    </div>


    ## Algorithms
    <div class="grid cards" markdown>

    - __20. Efficiency__ 

        Quantify the efficiency of an algorithm with big-O running time.
        [:octicons-arrow-right-24:](notes/20-efficiency.md)

    - __21. Searching__ 

        How to look for an element in a list.
        [:octicons-arrow-right-24:](notes/21-search.md)

    - __22. Sorting__ 

        How to rearrange a list of items into an order.
        [:octicons-arrow-right-24:](notes/22-sort.md)

    - __23. Tower of Hanoi__ 

        Solving the Tower of Hanoi recursively.
        [:octicons-arrow-right-24:](notes/23-tower.md)

    - __24. Permutation__

        Generate all possible permutations recursively.
        [:octicons-arrow-right-24:](notes/24-permutation.md)

    - __25. N-Queens__

        Solving the N-Queens problem, recursively.
        [:octicons-arrow-right-24:](notes/25-queens.md)

    </div>

    ## Intermediate C
    <div class="grid cards" markdown>

    - __26. Structure__ 

        Defining your own composite data type with `struct`
        [:octicons-arrow-right-24:](notes/26-struct.md)

    - __27. Standard C I/O__ 

        Use `printf` and `scanf` and their pitfalls
        [:octicons-arrow-right-24:](notes/27-stdio.md)
    </div>

=== ":fontawesome-solid-person-chalkboard: Lab Guide"

    ## CS1010 PE
    <div class="grid cards" markdown>

    - __CS1010 Programming Environment__

        We will be using a programming environment for CS1010 labs and practical exams.
        [:octicons-arrow-right-24:](guides/environments.md)

    - __Linking GitHub to CS1010 PE__

        We use GitHub Classroom for our labs.  Here is how to link your 
        GitHub account to your PE accounts.
        [:octicons-arrow-right-24:](guides/github.md)
    
    - __Using `tmate`__

        `tmate` is a tool that is useful for sharing terminal with others.
        [:octicons-arrow-right-24:](guides/tmate.md)


    </div>

    ## Vim
    <div class="grid cards" markdown>

    - __Philosophy of Vim__

        Understand the philosophy behind the design of Vim and why it is useful.
        [:octicons-arrow-right-24:](guides/vim-philosophy.md)

    - __Quick Vim Lessons__

        Learn the basic commands of Vim for navigation and editing.
        [:octicons-arrow-right-24:](guides/vim-quick-lessons.md)

    - __Vim Setup__

        Set up your Vim properly for CS1010.
        [:octicons-arrow-right-24:](guides/vim-setup.md)

    - __Vim Extensions__

        Learn about Vim extensions officially supported by CS1010.
        [:octicons-arrow-right-24:](guides/vim-plugins.md)

    - __Vim Tips for CS1010__

        Read about useful `vim` tips for CS1010.
        [:octicons-arrow-right-24:](guides/vim-tips.md)

    </div>

    ## Unix
    <div class="grid cards" markdown>

    - __Background__

        Understand the design principles of Unix and its power through its historical background.
        [:octicons-arrow-right-24:](guides/unix-background.md)

    - __Essential Unix Commands__

        Learn the essential commands to perform day-to-day operations in a Unix shell.
        [:octicons-arrow-right-24:](guides/unix-essentials.md)

    - __Advanced Unix Commands__

        Level-up your productivity when operating in a Unix shell.
        [:octicons-arrow-right-24:](guides/unix-advanced.md)

    </div>

    ## C
    <div class="grid cards" markdown>

    - __C in CS1010__

        C is a programming language with some nuances that get in the way of learning programming for beginners.   CS1010 bans and discourages some of these features.
        [:octicons-arrow-right-24:](guides/c-in-cs1010.md)

    - __CS1010 I/O Library__

        One of the ways CS1010 simplify C programming for beginners is to provide a library for robust reading and writing of common types without resorting to `printf` and `scanf`.  Read more about this `libcs1010` library here.
        [:octicons-arrow-right-24:](guides/library.md)

    - __C Style Guide__
      
        Read more about the expected coding style that CS1010 has to follow
        [:octicons-arrow-right-24:](guides/style.md)

    - __C Documentation Guide__
      
        Read more about the expected documentation format that CS1010 has to follow
        [:octicons-arrow-right-24:](guides/documentation.md)

    - __Compiling with `clang`__

        CS1010 hides much details about the use of `clang`, the C language compiler, behind the provided `Makefile`.  Students wishing to learn how to compile using `clang` can learn more here..
        [:octicons-arrow-right-24:](guides/clang.md)


    </div>

=== ":fontawesome-solid-keyboard: Programming Exercises"

    ## Exercise 0

    #### Week 3
    
    - [Questions](exercises/ex00.md)
    - [Accept Link](https://classroom.github.com/a/gkcRzoOm)

    | | Question | I/O     | Types            | Arithmetic | Function |  Difficulty |
    --|---------|----------|------------------|------------|----------|-------------|
    1 | [Echo](#question-1-echo)     | :material-check: | :material-check: |                    | | |
    2 | [Divide](#question-2-divide) | :material-check: | :material-check: | :material-check:   | | :material-star-half:    |
    3 | [Ones](#question-3-ones)     | :material-check: | :material-check: | :material-check:   | | :material-star-half:  |
    4 | [BMI](#question-4-bmi)       | :material-check: | :material-check: | :material-check:   | :material-check: | :material-star:      |
    5 | [Quadratic](#question-5-quadratic) | :material-check: | :material-check: | :material-check: | :material-check: | :material-star:       |
    6 | [Cuboid](#question-6-cuboid) | :material-check: | :material-check: | :material-check:   | :material-check: | :material-star: :material-star-half:  |

