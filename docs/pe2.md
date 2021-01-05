# Practice Exam 2

## Basic Info

- Date: 7 November 2020 (Saturday)
- Time: 9 am to 12noon (Report to invigilator at 9 am, exam starts at 9:30am)
- Venue: Online 
- Scope: Units 1-27, Assignments 1-8, Tutorials 1-10
- 5 programming questions: from very easy to very hard
- Criteria: correctness, style, efficiency, and documentation.  These are applied differently to different question (e.g., efficiency is important only for some questions, documentation is required only for some questions).
- Duration: 2 hours and 30 minutes
- Open Book (You can refer to printed/written materials, but no online resources are allowed).

## Special Restrictions

- You will be issued a special account for use on the day of the practical exam.   This will been sent to your NUS email account and you should will get a chance to test it during Thursday's tutorial session. 

- You will need to log into a special set of PE nodes through `ssh` to solve the exam questions.  

- You are not allowed to use the Internet for other purposes.  You are only allowed to (i) interact with the files on the PE nodes through `ssh`; and (ii) communicate with the invigilators through Zoom.  File transfer into the PE nodes is not allowed.  

## Vim Configuration

Your default account will have the same `.vimrc` as `~cs1010/.vimrc` on the CS1010 PE hosts.  

You are free to edit this during the practical exams.  

You, however, will not be able to download nor install `vim` plugins.

## E-Exam Procedure

We adopt the [E-exam procedure for the School of Computing](https://mysoc.nus.edu.sg/academic/e-exam-sop-for-students/) for CS1010.  This is a long document with a lot of information.  Please read through it carefully.  Please set up the software and hardware needed for taking e-exams before the test so that your test-taking experience is as smooth as possible.

Note the following supplementary information to complete the E-exam procedure above, specific to CS1010.

### 2.1 Exam Taking Software

   You will use a terminal to `ssh` into your allocated PE nodes to take the practical exam.

    You can only access the PE nodes for examination through either SoC VPN or tunnels through sunfire. 

### 2.2 Proctoring Protocol

   Note that the following applies to CS1010:

   - You are allowed to use only a single screen. If you’re using an external monitor, the laptop screen must be switched off.
   - The terminal running on your PC must be in full-screen mode.  Terminal window/tab other than the one used to connect to the PE nodes are not allowed.  
   - You are allowed only one full-screen terminal window.  You may use split windows within `vim` to view the exam questions and your code side-by-side.
   - If you need to switch to other Windows (e.g., Zoom chat to ask question), you need to get permission from the invigilator.
   - {++ Do note that failure to comply with proctoring requirements may require you to retake PE2 ++}

### 2.4. Seeking Clarifications on Exam Questions

   You are allowed to ask clarification questions during the exam.  However, you may only ask a boolean yes/no question.  For instance, you are not allowed to ask "What can we assume about the input?".  You should rephrase it as "Can we assume that the input is always positive?".  Our answers will only be in the form of "yes", "no", "no comment".

### 2.5. Multi-part Exam

   There is only a single part with no break in between.

### 9.6. Completion of Exam

   The folder to submit the your recording to is LumiNUS> CS1010 > Files > PE2 Screen Capture Video Submission.  You should submit it no later than 7 November 2020, 2359.  Note that failure to submit equates to you not being proctored for Practical Examination 2.
 
## Zoom Session Assignment

- [Group assignment](https://luminus.nus.edu.sg/modules/c7b362a0-6aee-4b22-b4da-f9e8074249fd/groups/class-groups/c9273adc-94e7-4c08-90d9-a764182aea6c) is available on Luminus.

- {++ Zoom sessions are available on [Piazza](https://piazza.com/class/kdgunoizhic105?cid=693) ++}

## Invigilators

TBD

## Emergency Contact (Examination issues)

- Zoom Chat (when permission is given by the proctor)
- If Zoom fails, MS Teams (when permission is given by the proctor)
- If Zoom/Teams fail, as a last resort, you may email the instructors/proctors
- Find Proctor and Instructor Email Information on LumiNUS > CS1010 > Module Details > Facilitators (Top Menu) (note down the email before the exam in case Luminus fails)

##  Emergency Contact for Technical Issues 

- School of Computing - Technical Services
- Telephone: 6516 2736
- Email: techsvc@comp.nus.edu.sg
- Emergency Telephone: 6874 2736 (available only during emergencies and network outages)

## General Advice

- Save your program regularly.  We will use setup every account with `~/.vimrc` copied from `~cs1010/.vimrc`.  Thus, you can find the last saved version of your files under `~/.backup` if you accidentally deleted your code.
- Plan your time properly.  Do not spend excessive time on any task.  Read through all questions and solved those that you are confident to solve first.
- There are five questions, from very easy to very hard.  Solve as many as you can.  I expect most students will be able to solve 3 out of the 5 questions within the time limit.
- There is one mark allocated to style for each question.  As long as you keep your code clean, neat, and readable, you will get this one mark, almost for free.  Review the CS1010 style guide so that you know what is expected in terms of coding style.
- Don't start typing your code right away.  Think about the solution first -- what variables are needed?  What is the control flow (using branches and loops)?  Draw out the flowchart if it helps.  
- Break down the problem into smaller ones if the problem is too complex to solve.
- You are not allowed to start typing on the computer until the invigilator announced that you can do so.
- Just like the assignments, you are not given all the test cases that we will be using during grading.  Please test your code against additional test cases, especially for boundary cases.

## Practice Paper

You can use the PE question from 18/19 Semester 1 as the practice paper.

- Download the [question paper](https://www.comp.nus.edu.sg/~ooiwt/cs1010/1819s1/pe2.pdf)
- [Accept the assignment](https://classroom.github.com/a/o4OHFC_V) on GitHub
- Run `~cs1010/get-pe19` on any PE host to get the skeleton code and test cases
- Run `~cs1010/submit-pe19` to submit/archive your solution on GitHub

