# Assignment 2: Collatz, Rectangle, Prime, Pattern

### Deadline

13 September 2022 (Tuesday), 23:59 pm.

### Prerequisite

- Solve [Exercise 3](ex03.md).

### Learning Objectives

- Be comfortable writing C programs that involve loops with while/for/do-while statements.
- Be able to write C programs that are neat and readable, adhering to a common prescribed coding convention.

### Grading

This assignment contributes to 3% of your final grade.  The total marks for this assignment are 30 marks.  

For Assignment 2, we may deduct up to 1 mark for style violation.  Ensure that your code is neat and readable, adhering to the CS1010 coding convention.

1 mark is allocated to efficiency for `prime` and `pattern`.  While we do not require an advanced fast algorithm to check for primality testing, your code should not waste time by doing redundant work.  You can use the `time` command to measure how long it takes to run a command.  E.g.,

```
$ time ./test.sh prime
prime: passed

real    0m3.908s
user    0m3.864s
sys     0m0.083s

$ time ./test.sh pattern
pattern: passed

real    0m0.172s
user    0m0.133s
sys     0m0.059s
```

The rest of the marks are allocated to correctness -- this includes the correctness of syntax, practices, approach, and logic, and whether you correctly follow our instructions.  Note that _even if your solution produces the correct output every time, it may not get full marks if the approach is wrong._

## Question 1: Collatz (5 marks)

The Collatz Conjecture was introduced by the mathematician [Lothar Collatz](https://en.wikipedia.org/wiki/Lothar_Collatz) in 1937. Also known as the $3n+1$ conjecture, the problem can be stated very simply but yet no one can prove that it is true or false. The conjecture states the following:

Consider the following operation on a positive integer $n$: if $n$ is even, divide it by two; otherwise, triple it and add one. Suppose we form a sequence of numbers by performing this operation repeatedly, beginning with any positive integer, then this process will eventually reach the number 1, for any initial positive integer $n$.

For instance, if $n$ = 10, then we have the sequence

10 -> 5 -> 16 -> 8 -> 4 -> 2 -> 1

The smallest number of steps taken by this process for $n$ to reach 1 is called the _total stopping time_. In the example above, the total stopping time for 10 is 6.

Write a program `collatz.c` that reads in two positive integers $m$ and $n$ from the standard input ($m \le n$) and finds out, among the numbers between $m$ to $n$, inclusive, which one has the largest total stopping time. If two numbers have the same total stopping time, we break ties by choosing the larger number as the answer.

Your program should print to the standard output, the largest total stopping time, followed by the corresponding number, in two different lines.

### Sample Run

```
ooiwt@pe114:~/as02-skeleton$ ./collatz
1 1
0
1
ooiwt@pe114:~/as02-skeleton$ ./collatz
1 9
19
9
```

## Question 2: Rectangle (5 marks)

Write a program called rectangle that reads two positive integers from the standard input, corresponding to the width and the height of the rectangle. The width and height must be at least 2. Draw a rectangle on the screen using the special ASCII characters "╔" "╗" "╝" "╚" "═" "║", which corresponds to the top left, top right, bottom right, bottom left, top/bottom edge, and left/right edge of the rectangle respectively. Strings consisting of these special characters have been given to you in `rectangle.c`, and we have defined them as constants. For instance, "╔" is called `TOP_LEFT`, and to print this out, you can write 
``` 
cs1010_print_string(TOP_LEFT);
```

### Sample Run

```
ooiwt@pe113:~/as02-ooiwt$ ./rectangle
2 2
╔╗
╚╝
ooiwt@pe113:~/as02-ooiwt$ ./rectangle
2 10
╔╗
║║
║║
║║
║║
║║
║║
║║
║║
╚╝
ooiwt@pe113:~/as02-ooiwt$ ./rectangle
10 10
╔════════╗
║        ║
║        ║
║        ║
║        ║
║        ║
║        ║
║        ║
║        ║
╚════════╝
```

## Question 3: Prime (5 marks)

Write a program called `prime` that reads a positive integer $n$ ($n \ge 2$) from the standard input and prints the largest prime smaller or equal to $n$.

Recall that a prime number is a number that is only divisible by 1 and itself.

Your program must not make unnecessary checks or do repetitive work.  In particular, once you found evidence that a number is not a prime, there is no need to continue checking.

Your program must contain a boolean function `is_prime` that checks if a given number is prime.  You should call this function in a loop to solve this problem.

As a reference, `prime` should not take more than 5 seconds on CS1010 PE hosts to pass all the provided test cases.

### Sample Run

```
ooiwt@pe113:~/ex03-ooiwt$ ./prime
2
2
ooiwt@pe113:~/ex03-ooiwt$ ./prime
10
7
ooiwt@pe113:~/ex03-ooiwt$ ./prime
1000
997
```

## Question 4: Pattern (15 marks)

Even though the sequence of prime numbers appears to be random, mathematicians have found some intriguing patterns related to prime numbers. In this question, you are asked to write a program to draw a variation of the "Parallax Compression" pattern discovered by a software engineer, Shaun Gilchrist.

The pattern visualizes the distribution of prime numbers in a triangle, in the following way. The inputs given are an interval $m$ ($m \ge 1$) and the height of the triangle $h$.

The triangle has $h$ rows. The first row of the triangle has one cell, the second row has three cells, the third row has five, etc. The cells are centrally aligned so that visually they form an equilateral triangle. We call the left-most cell of each row the leading cell.

Each cell in the triangle contains $m$ integers. The first cell in the first row contains the numbers 1, 2, ..., $m$. The leading cell of the next row, Row 2, contains $m$ numbers between $m + 1$ and $3m$, with an increment of 2: i.e., $m + 1$, $m + 3$, $m + 5$, .., $m + (2m - 1)$. The leading cell of the next row, Row 3, contains the numbers $3m + 1$ and $6m$, with increment of 3: i.e., $3m + 1$, $3m + 4$, $3m + 7$,..  $3m+(3m−2)$,
etc.

!!! tips "Lemon Run"
    The leading cell of the rows corresponds to lemon runs for $(1, m, 1)$, $(m+1, m, 2)$, $(3m+1, m, 3)$, etc.  See [Exercise 3](ex03.md)

For instance, if $m$ is 5, the leading cells of the first three rows contain the numbers

- {1, 2, 3, 4, 5},
- {6, 8, 10, 12, 14},
- {16, 19, 22, 25, 28},

respectively.

The rest of the cells in each row contains $m$ numbers where each is one more than a number contained in the cell on its left. So, in Row 2, the numbers in the three cells are

- {6, 8, 10, 12, 14},
- {7, 9, 11, 13, 15}, and
- {8, 10, 12, 14, 16}.

In Row 3, the cells contain

- {16, 19, 22, 25, 28},
- {17, 20, 23, 26, 29},
- {18, 21, 24, 27, 30},
- {19, 22, 25, 28, 31}, and
- {20, 23, 26, 29, 32}.

Now, to visualize the distribution of primes, we do the following, for each cell of the triangle that contains at least one prime, we print `#` to the standard output at the corresponding position. Otherwise, we print `.`.


For example, in Row 3,

- Cell 1: {16, 19, 22, 25, 28}, we print `#` since 19 is prime
- Cell 2: {17, 20, 23, 26, 29}, we print `#` since 23 is prime
- Cell 3: {18, 21, 24, 27, 30}, we print `.` since there is no prime
- Cell 4: {19, 22, 25, 28, 31}, we print `#` since 19 is prime
- Cell 5: {20, 23, 26, 29, 32}, we print `#` sine 23 is prime

So Row 3 will be printed as `##.##` (leading and trailing white spaces are not shown).

Your output must contain exactly $h$ rows, each row exactly $2h−1$ characters (including the white spaces but excluding the newline). Note that in the sample runs below, the white spaces are not visible.

### Example

```
ooiwt@pe114:~/as02-skeleton$ ./pattern
2 4
   #   
  #.#  
 ##.## 
#.#.#.#
```

To understand the output, consider the cells below:

![pixel](figures/as02-pattern/pixels-1.png)

Now, consider the two numbers contained in each cell, in the four rows:

- Row 1: {1,2}
- Row 2: {3,5} {4,6} {5,7}
- Row 3: {7,10} {8,11} {9,12} {10,13} {11,14}
- Row 4: {13,17} {14,18} {15,19} {16,20} {17,21} {18,22} {19,23}

![pixel](figures/as02-pattern/pixels-2.png)

Now, we check whether the numbers contained in each cell have at least one prime, and replace them with either `#` or `.`.

![pixel](figures/as02-pattern/pixels-3.png)

### Sample Runs

```
ooiwt@pe114:~/as02-skeleton$ ./pattern
11 11
          #          
         .#.         
        ##.##        
       #.#.#.#       
      ####.####      
     .#.#...#.#.     
    ######.#.####    
   #.#.#.#.#.#.#.#   
  ##.##.##.##.##.##  
 .#.#.#.#...#.#.#.#. 
######.###.##########
ooiwt@pe114:~/as02-skeleton$ ./pattern
100 29
                            #                            
                           #.#                           
                          ##.##                          
                         #.#.#.#                         
                        ####.####                        
                       #...#.#...#                       
                      ######.######                      
                     #.#.#.#.#.#.#.#                     
                    ##.##.##.##.##.##                    
                   #.#...#.#.#.#...#.#                   
                  ##########.##########                  
                 #...#.#...#.#...#.#...#                 
                ############.############                
               #.#.#...#.#.#.#.#.#...#.#.#               
              ##.#..##..#.##.##.#..##..#.##              
             #.#.#.#.#.#.#.#.#.#.#.#.#.#.#.#             
            ################.################            
           #...#.#...#.#...#.#...#.#...#.#...#           
          ##################.##################          
         #.#...#.#.#.#...#.#.#.#...#.#.#.#...#.#         
        ##.##..#.##.#..##.##.##.##..#.##.#..##.##        
       #.#.#.#.#...#.#.#.#.#.#.#.#.#.#...#.#.#.#.#       
      ######################.######################      
     #...#.#...#.#...#.#...#.#...#.#...#.#...#.#...#     
    ####.####.####.####.####.####.####.####.####.####    
   #.#.#.#.#.#...#.#.#.#.#.#.#.#.#.#.#.#...#.#.#.#.#.#   
  ##.##.##.##.##.##.##.##.##.##.##.##.##.##.##.##.##.##  
 #.#.#...#.#.#.#.#.#...#.#.#.#.#.#...#.#.#.#.#.#...#.#.# 
############################.############################
```

### Hints

As always, solve this problem by breaking it down into smaller problems.

In addition to drawing triangles and checking if a number is prime, you might find the following sub-problems useful:

- Find the first number of each leading cell of each row, given the row number and the interval $m$.

- Given the row, the col, and the interval $m$, does the cell contain a prime?

`pattern` should take less than a second to pass all the given test cases.
