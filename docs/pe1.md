# Practice Exam 1

## Basic Info

- Date: 2 October 2021 (Saturday)
- Time: 9 am to 12noon (Report to invigilator at 9 am, exam starts at 9:30am)
- Venue: Online (except those already informed)
- Scope: Units 1-12, Assignments 1-2, Exercises 1-4
- 5 programming questions: from very easy to very hard
- Criteria: correctness and style.  There will be one question where efficiency matters. 
- Duration: 2 hours and 30 minutes
- Open Book (You can refer to printed/written materials, but no online resources are allowed).

## Practice Paper

- [Exercise 6](ex06.md)
- Exercise 7

## Special Restrictions

- You will be issued a special account for use on the day of the practical exam.   This will be sent to your NUS email account and you will be able to test it during Week 7's lab session. 

- You will need to log into a special set of PE nodes through `ssh` to solve the exam questions.  

- You are not allowed to use the Internet for other purposes.  You are only allowed to (i) interact with the files on the PE nodes through `ssh`; and (ii) communicate with the invigilators through Zoom.  File transfer into the PE nodes is not allowed.  

## Vim Configuration

Your default account will have the same `.vimrc` as `~cs1010/.vimrc` on the CS1010 PE hosts.

You are free to edit this during the practical exams.

You, however, will not be able to download `vim` plugins.  You can only install from a list of [approved `vim` plugins and installation instructions](https://nus-cs1010.github.io/2122-s1/vim.html#7-approved-plugins).

After loggin in, you will be given 5 minutes to configure your `~/.vimrc` environment and set up allowed plugins.

## General Advice

- Save your program regularly.  We will use setup every account with `~/.vimrc` copied from `~cs1010/.vimrc`.  Thus, you can find the last saved version of your files under `~/.backup` if you accidentally deleted your code.
- Plan your time properly.  Do not spend excessive time on any task.  Read through all questions and solved those that you are confident to solve first.
- There are five questions, from very easy to very hard.  Solve as many as you can.  I expect most students will be able to solve 3 out of the 5 questions within the time limit.  You don't need to solve all questions to deserve an A grade.
- There is one mark allocated to style for each question.  As long as you keep your code clean, neat, and readable, you will get this one mark, almost for free.  Review the CS1010 style guide so that you know what is expected in terms of coding style.
- Don't start typing your code right away.  Think about the solution first -- what variables are needed?  What is the control flow (using branches and loops)?  Draw out the flowchart if it helps.  
- Break down the problem into smaller ones if the problem is too complex to solve.
- You are not allowed to start typing on the computer until the invigilator announced that you can do so.
- Just like the assignments, you are not given all the test cases that we will be using during grading.  Please test your code against additional test cases, especially for boundary cases.

## E-Exam Procedure

We adopt the [E-exam procedure for the School of Computing](https://mysoc.nus.edu.sg/academic/e-exam-sop-for-students/) for CS1010.  This is a long document with a lot of information.  Please read through it carefully.  Please set up the software and hardware needed for taking e-exams before the test so that your test-taking experience is as smooth as possible.

Note the following supplementary information to complete the E-exam procedure above, specific to CS1010.

### 2.1 Exam Taking Software

   You will use a terminal to `ssh` into your allocated PE nodes to take the practical exam.

   You can only access the PE nodes for examination via SoC VPN or through SoC network.

### 2.2 Proctoring Protocol

   Note that the following applies to CS1010:

   - You are allowed to use only a single screen. If you’re using an external monitor, the laptop screen must be switched off.
   - The terminal running on your PC must be in full-screen mode.  Terminal window/tab other than the one used to connect to the PE nodes are not allowed.  
   - You are allowed only one full-screen terminal window.  You may use split windows within `vim` to view the exam questions and your code side-by-side.
   - If you need to switch to other Windows (e.g., Zoom chat to ask question), you need to get permission from the invigilator.

### 2.4. Seeking Clarifications on Exam Questions

   You are allowed to ask clarification questions during the exam.  However, you may only ask a boolean yes/no question.  For instance, you are not allowed to ask "What can we assume about the input?".  You should rephrase it as "Can we assume that the input is always positive?".  Our answers will only be in the form of "yes", "no", "no comment".

### 2.5. Multi-part Exam

   There is only a single part with no break in between.

### 12. Screen Recording Software

   We recommend Penopto for screen recording.


## Zoom Session Assignment

- Zoom session assignment is available internally via [LumiNUS](https://luminus.nus.edu.sg/modules/600de8e9-67ec-47dd-b597-a2eeadb45792/groups/class-groups/6dccb622-bd40-481e-addd-655683f44956?listView=false)

- Zoom links to be made available via Piazza (two days before the PE1)

## Invigilators and Email Address

```
X01	Guo Ai			guo.ai@u.nus.edu
X02	Luo Xinjian		xinjian.luo@u.nus.edu
X03	Alvin Heng		alvin.heng@u.nus.edu
X04	Song Kai		song.kai@u.nus.edu
X05	Eric Bryan		e0555789@u.nus.edu
X06	Tean Wei Jun	e0540193@u.nus.edu
X07	Liang Yuzhao	e0543802@u.nus.edu
X08	Felix Halim		e0407645@u.nus.edu
X09	Xia Fuxi		e0426189@u.nus.edu
X10 Ling Yan Hao	e0174827@u.nus.edu
X11 Adi Nata		e0425080@u.nus.edu
X12 Wang ChengXin	e0673190@u.nus.edu
```

## Emergency Contact (Examination issues)

- Zoom Chat (when permission is given by the proctor)
- If Zoom fails, use MS Teams (when permission is given by the proctor)
- If Zoom/Teams fail, as a last resort, you may email the instructors/proctors

##  Emergency Contact (Technical Issues)

- School of Computing - Technical Services
- Telephone: 6516 2736
- Email: techsvc@comp.nus.edu.sg
- Emergency Telephone: 6874 2736 (available only during emergencies and network outages)
