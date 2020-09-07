# Guide to Programming Assignments

## Timeline

There will be weekly take-home programming assignments, each consisting of 1 to 4 questions. 
These programming assignments collectively contribute to 30% of your final grade.

The programming assignment is released on the CS1010 website every Thursday, with a deadline given.  You must submit all questions for each particular programming assignment before the deadline.

## General Advice
You are advised to (i) spend some time thinking before you begin coding, (ii) practice incremental coding and (iii) test your programs thoroughly.

Remember to spend some time thinking about the algorithm for each question.  Write out the pseudo-code or draw out the flowchart on your own before you start typing in your program. 

Incremental coding means do NOT type in the whole long program in a single session and then compile it. Instead, type your program in bits and pieces and compile it incrementally. Try to maintain a compilable program even while you are working on it. Submitting a compilable program that partially works is better than submitting an un-compilable one; this is especially important for your practical exams. 

You should test your program thoroughly with your own test data before submission.  

Please note that:

- You may assume that all input data are correct unless otherwise stated. Hence you do NOT need to do input data validation. This is to allow you to focus on getting the program right, instead of worrying about making your program fool-proof which involves a lot more work. 

- Copying others' programs or relying on others to help you with these assignments will only offer a short-term reprieve. When Practical Exam (PE) time comes, your inadequacy will be exposed and the consequence would be dire.

## Late Submission
All programming assignments must be submitted on time.  If you need an extension, please ask for one and provide a justification for approval.  Only academic reasons and compassionate reasons can be considered (e.g., representing NUS for a sports event is OK; Attending a wedding is not.)

For late submission, there is a 1% penalty (of the total awarded marks for that particular assignment) for every 5-minute after the deadline, capped at 80%.  For example, if an assignment is awarded 40 marks, and it is submitted 100 minutes after the deadline, the student will get 32 marks instead (20% penalty).  If it is submitted 10 hours after the deadline, the student will get 8 marks (as it has hit the cap of 80% penalty).

## Method of Submission
Please follow the instructions provided in each programming assignment to submit the programs to GitHub.  Programs submitted through other means, such as emails, will NOT be accepted.

## Identifying Yourself

In every C file that you submit to CS1010, you need to identify yourself by writing your name and tutorial group. Marks will be deducted if you fail to do so. You need to edit the line:

```
@author XXXX (Group YYYY)
```

and change it to something like:

```
@author Gamora (Group 10)
```

Please follow the instructions provided in each programming assignment to submit the programs to GitHub.  Programs submitted through other means, such as emails, will NOT be accepted.

## Grading
Only the final submission of each assignment will be graded.  For each assignment, we will provide you will a limited set of test data.  During grading, we may grade your program with additional test data.

Each programming assignment will be graded differently.  Generally, marks are given for attempt, correctness, design, and style, and documentation.  The weight of each one will be adjusted over the semester.  

A program that cannot compile (i.e., there is a compilation error) will receive 0 marks for correctness.

In addition, there is an additional -1 mark for each warning that your program received.  Always make sure that your program compiles cleanly without any warning.

Feedback will be provided by the tutors on GitHub.

## Use of Piazza
If you have doubts about the problem statements of an assignment, you may raise them on Piazza.  But before that, please read through the problem statements carefully first, and check if the same questions have been asked and answered on the forum.

Please exercise discretion when posting to Piazza.  

Before the deadline, you are NOT to post the solution to the assignment, complete or partial, on Piazza (or any publicly accessible online site).

## Disallowed Syntax
Some programming assignments may explicitly disallow the use of certain syntax. Generally, using syntax or statements which are not yet covered either in class or in the assignment statement is strongly discouraged.  We also discourage the use of certain syntax for this module, (e.g., `++`) you should not use them.  The assignments are designed such that you should not need to do so (even though doing so may result in your program being shorter or more efficient). If the objective of the assignment is undermined, the penalty for using such forbidden syntax will be heavy. If in doubt, please ask for clarification.

You can find the list of banned and discouraged C features in the article "[C in CS1010](c-in-cs1010.md)."

## Plagiarism
You are NOT to copy from others or allow others to copy your programs.  We take plagiarism seriously.  See [our policies](policies.md) page for details.

This means that you should also guard your solution carefully, not posting them to publicly accessible places, or change the permissions of the files on the CS1010 PE hosts so that it is accessible by others.

---
This guideline is adapted from Aaron Tan's CS1010 guideline.
