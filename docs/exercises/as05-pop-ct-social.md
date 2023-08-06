# Assignment 5: Popular, Contact, Social

### Deadline

18 October 2022 (Tuesday), 23:59 pm.

### Prerequisite

- Solve [Exercise 10](ex10-echo-add-line.md)

### Learning Objectives

- Writing C programs that use 2D arrays
- Manage memory allocation and deallocation correctly
- Processing and manipulating data from a jagged 2D array

### Grading

This assignment contributes to **4.5%** of your final grade.  The total marks for this assignment are 45 marks.  

For Assignment 4, we may deduct up to 1 mark _for each question_ for style violation.  Ensure that your code is neat and readable, adhering to the CS1010 coding convention.

Furthermore, 2 mark is allocated to documentation. Your code should follow the good practices of breaking down your solution into small functions. Document each function (except `main`) following the Doxygen documentation format, as outlined [here](../guides/documentation.md).  Your code must have at least one non-trivial function (other than `main()`) to be eligible for these 2 marks.

The rest of the marks are allocated to correctness -- this includes the correctness of syntax, practices, approach, and logic, and whether you correctly follow our instructions.  Note that _even if your solution produces the correct output every time, it may not get full marks if the approach is wrong._

## Question 1: Popular (10 Marks)

We can measure the popularity of a person by how many friends a person has.  We assume that "friend" is a symmetric relation. If A is a friend of B, then B is a friend of A.  Because of this, we can represent friendships between people as a lower triangular matrix (using jagged 2D array). A proper type to store in each element of the matrix is bool. To simplify our life, however, we store each element of the matrix as a char, with '1' representing a friendship connection, '0' otherwise. The friendship for $n$ people is thus an array of $n$ strings, each string containing characters of '0' and '1' only. The first row of the matrix is a string of length one; the second row is of length two; the third row, length three, etc. The last character of each string (i.e., the diagonal of the matrix) is 1 as we assume everyone is a friend with him/herself.

For instance, suppose we have the following friendship relations.  We represent each person with id 0, 1, 2, ... Person with id $i$ has its information stored in Row $i$ and Column $i$. Recall that if Row $i$ and Column $j$ is 1, it means that Person $i$ is a friend of Person $j$.

```
1
01
011
```

The relation indicates that Person 1 and 2 are friends with each other. Person 0 is not a friend of 1 and 2. 

The example below add another Person 4, who is a friend of Person 0 and Person 2.
```
1
01
011
1011
```

Write a program `popular`, that reads from standard input:

- a positive integer $n$,
- followed by $n$ lines of strings consisting of '1' or '0' representing the contact traces of these $n$ people.

Print, to the standard output, 

- the id of the person who is the most popular (i.e., with the most number of friends).  We break ties by preferring the person with a smaller id.
- the number of friends that the person has.

The purpose of this question is for you to practice using a jagged 2D array. Hence, you are not allowed to store the input matrix or intermediate matrices using a rectangular array, or you risk being penalized heavily for this question.

### Sample Runs

```
ooiwt@pe119:~/as05-skeleton$ cat inputs/popular.1.in
3
1
01
011
ooiwt@pe119:~/as05-skeleton$ ./popular < inputs/popular.1.in
1
1
ooiwt@pe119:~/as05-skeleton$ cat inputs/popular.2.in
4
1
01
011
1011
ooiwt@pe119:~/as05-skeleton$ ./popular < inputs/popular.2.in
2
2
```

## Question 2: Contact (15 marks)

Contact tracing is an important step to contain a pandemic. Suppose we are given the information on who is in contact with who. We wish to answer the following question:

- Given two people A and B, have they been directly in contact with each other?
- Given two people A and B, is there a common person that they have been in contact with? In other words, is there a third person C, where (i) A and C have been in contact with each other, and (ii) B and C have been in contact with each other?

We assume that "contact" is a symmetric relation. If A has been in contact with B, then B has been in contact with A too. Because of this, we can represent the contact traces between n people as a lower triangular matrix (using a jagged 2D array). Similar to Question 1, we store each element of the matrix as a char, with '1' representing a contact, '0' otherwise. The contact traces for $n$ people is again an array of $n$ strings, each string containing characters of '0' and '1' only. The first row of the matrix is a string of length one; the second row is of length two; the third row, length three, etc. The last character of each string (i.e., the diagonal of the matrix) is 1 since everyone has contact with him/herself.

Write a program contact, that reads from standard input:

- a positive integer $n$,
- followed by $n$ lines of strings consisting of '1' or '0' representing the contact traces of these $n$ people.
- followed by two positive integers $j$ and $k$, representing the ids of a pair of people we are interested in querying. An input of $i$ corresponds to the person whose contact information is stored in Row $i$ and Column $i$.

Print, to the standard output, the following information:

- "direct contact" if there is direct contact between $j$ and $k$
- "contact through x" if there is no direct contact but there is an indirect contact between $j$ and $k$ through a third person $x$ (replace $x$ with the actual id). If there are multiple such people, output the person with the smallest id.
- "no contact" if there is no direct nor indirect contact through a third person in the contact traces.

The purpose of this question is for you to practice using a jagged 2D array. Hence, you are not allowed to store the input matrix or intermediate matrices using a rectangular array, or you risk being penalized heavily for this question.

### Sample Runs

```
ooiwt@pe119:~/as05-skeleton$ cat inputs/contact.1.in
3
1
11
011
0 1
ooiwt@pe119:~/as05-skeleton$ ./contact < inputs/contact.1.in
direct contact
ooiwt@pe119:~/as05-skeleton$ cat inputs/contact.2.in
3
1
11
011
0 2
ooiwt@pe119:~/as05-skeleton$ ./contact < inputs/contact.2.in
contact through 1
ooiwt@pe119:~/as05-skeleton$ cat inputs/contact.3.in
4
1
01
011
1011
0 1
ooiwt@pe119:~/as05-skeleton$ ./contact < inputs/contact.3.in
no contact
```

## Question 3: Social (20 marks)

You should solve this question after solving Questions 1 and 2. The functions you wrote for Questions 1 and 2 might be useful for solving this question.

The idea of six degrees of separation states that everyone in the world is connected to every other person by at most 6 hops. Suppose that a person A is a friend of a person B, then we say that A is one hop away from B. Consider a friend of B, say C, who is not a friend of A. So C is a friend of a friend of A. We say that C is two hops away from A. Six degrees of separation generally means there is a chain of friendship that can connect any two people in the world with no more than 6 hops.

In this question, we are going to compute the chain of friendships up to the $k$ degree. Suppose there are $n$ people, and we know the social network of these $n$ people -- i.e., we know who is a friend with whom. Write a program social to compute a social network representing who is connected to whom via a friendship chain of degree $k$.

Similar to Question 1, we assume that friendship is bi-directional -- if A is a friend of B, then B is a friend of A. We represent a social network as a lower triangular matrix (using a jagged 2D array) in the same format as Question 1, where a 1 in Row $i$ and Column $j$ means Person $i$ and Person $j$ are friends; 0 otherwise.

The social network below shows the friendship relations between four people.

```
1
01
011
1011
```

Suppose now we consider the social network of degree 2. Person 0 is two hops away from Person 2 (Person 0 knows Person 3, and Person 3 knows Person 2). Person 1 is also two hops away from Person 3 (Person 1 knows Person 2 and Person 2 knows Person 3).

The social network of degree 2 becomes:

```
1
01
111
1111
```

Write a program `social, that reads from standard input two positive integers $n$ and $k$, followed by $n$ lines of strings consisting of '1' or '0' representing the social network of these $n$ people. Print, to the standard output, the social network formed by a friendship chain of degree $k$.

The purpose of this question is for you to practice using a jagged 2D array. Hence, you are not allowed to store the input matrix or intermediate matrices using a rectangular array, or you risk being penalized heavily for this question.
Hint

There are many ways to solve this problem. The most straightforward way is to compute the social network formed by a friendship chain of degree $i$, $N(i)$, changing $i$ from 1 to $k$.

- $N(1)$ is just the input;
- $N(2)$ can be computed in a similar way to how you solved Question 2.
- Now, given $N(i-1)$ and $N(1)$, can you compute $N(i)$?

### Sample Runs

```
ooiwt@pe119:~/as05-skeleton$ cat inputs/social.1.in
3 1
1
11
011
ooiwt@pe119:~/as05-skeleton$ ./social < inputs/social.1.in
1
11
011
ooiwt@pe119:~/as05-skeleton$ cat inputs/social.2.in
4 2
1
01
011
1011
ooiwt@pe119:~/as05-skeleton$ ./social < inputs/social.2.in
1
01
111
1111
ooiwt@pe119:~/as05-skeleton$ cat inputs/social.3.in
5 2
1
11
011
0011
10011
ooiwt@pe119:~/as05-skeleton$ ./social < inputs/social.3.in
1
11
111
1111
11111
```
