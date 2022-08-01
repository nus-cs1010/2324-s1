# Basic UNIX Commands

UNIX-based operating systems provide a command line to interact with the system: to create directories, to manipulate files, to run certain applications.  While these tasks can be done through a GUI interface, a command line is still useful because (i) it provides a simple way to interact with a remote system (only text are exchanged between the local and remote hosts instead of graphics); (ii) it is extremely flexible and provides many customization options; (iii) you can easily automate the sequence of commands to issue for repetitive tasks; and (iv) it is faster.  

## Connect to the Programming Environment (PE)

If you would like to follow the following examples, you should first `ssh` into one of the PE hosts provided.  We will use `pe111` in the following example.  But feel free to use `pe112` up to `pe120` for your practice.  You should read [this guide](environments.md) to see how to access and connect to the environment.


Once you are connected, you should see a prompt like this.

```
ooiwt@pe111:~$
```

This interface is provided by a UNIX bash -- this shell sits in a loop and waits for users to enter a command, then it interprets and executes the command.  There are many versions of shells, the default shell for our PE is `bash`[^1].


[^1]: I run `fish` on my macOS, as you might have noticed during the in-class demos.  You can use any shell you like if you know what you are doing.  Otherwise, `bash` is a popular one.

_The following is adapted for CS1010 from [the instructions created by Aaron Tan](http://www.comp.nus.edu.sg/~cs1020/4_misc/cs1010_lect/2014/intro_lab/gettingStarted.html). Bugs are mine._  

The power of UNIX stems from the many commands it offers. The following are a few commonly used commands. This list is by no means exhaustive, and you are urged to explore on your own. Note that UNIX commands are _case-sensitive_.

All commands are to be entered after the UNIX prompt of the form

```
ooiwt@pe111:~$
```

`~` indicates that you are currently in your home directory.  The following examples assume that user `ooiwt` is logged into `pe111`.

It might be good to understand the directory structure in UNIX, a multi-user system. The directory tree is shown below:

![UNIX Directory Tree](figures/unix-dir-tree.png)

Each user has his/her own home directory, which is where he/she will be automatically placed when he/she logs in to the system. The above figure shows where the home directory of the user `ooiwt` resides in the directory tree. The user `ooiwt` may create files or directories in his/her home directory, but not elsewhere unless permission is given.

## `pwd`: Print Current Working directory

`pwd` shows you which directory you are currently in
```
ooiwt@pe111:~$ pwd
/home/o/ooiwt
```

UNIX uses forward slash `/` to delimit different parts of the directory structure.  This is the same notation as URLs, so you should already be familiar with it.

## `ls`: LiSt files

The `ls` list the files in the current working directory.

```
ooiwt@pe111:~$ ls
ooiwt@pe111:~$
```

If you do not have any regular files in your home directory, as you should when you first log in, you should immediately return to the bash prompt.  

!!! note "Rule of Silence"
    UNIX follows the _rule of silence_: programs should not print unnecessary output, to allow other programs and users to easily parse the output from one program.  So, if `ls` has nothing to list, it will list nothing (as opposed to, say, printing "This is an empty directory.")

## `mkdir`: MaKe a subDIRectory

The `mkdir` command creates a subdirectory with the given name in the current directory.

```									 	
ooiwt@pe111:~$ mkdir tut01
ooiwt@pe111:~$ ls
tut01
ooiwt@pe111:~$ ls -F
tut01/
```

Here, you create a directory called `tut01`.  Now, when you `ls`, you can see the directory listed.

You may also use `ls -F` for more information (`-F` is one of the many _options_/_flags_ available for the `ls` command. To see a complete list of the options, refer to the man pages, i.e., `man ls`.)

The slash `/` beside the filename tells you that the file is a directory (aka folder in Windows lingo). A normal file does not have a slash beside its name when "ls -F" is used.

You may also use the `ls -l` command (hyphen el, not hyphen one) to display almost all the file information, include the size of the file and the date of modification.

!!! tip "Use ++control+p++ for Command History"
    Unix maintains a history of your previously executed UNIX commands, and you may use ++control+p++ and ++control+n++ to go through it. Press the ++control+p++ until you find a previously executed UNIX command. You may then press ++enter++ to execute it or edit the command before executing it. This is handy when you need to repeatedly execute a long UNIX command.

## `cd`: Change Directory

To navigate in the directory tree, changing the current working directory from to another, we use the `cd` command.

```
ooiwt@pe111:~$ cd tut01
ooiwt@pe111:~/tut01$
```
Note that the prompt changes to `~/tut01` to indicate that you are now in the `tut01` directory below your `HOME` directory.

Entering `cd` alone brings you back to your `HOME` directory, i.e., the directory in which you started with when you first logged into the system.
```
ooiwt@pe111:~/tut01$ cd
ooiwt@pe111:~$
```

Two dots `..` refers to the parent directory.  So, alternatively, for the case above, since we are only one level down from the `HOME`, to return home, we can alternatively use `cd ..`.

```
ooiwt@pe111:~/tut01$ cd ..
ooiwt@pe111:~$
```

## `rmdir`: ReMove a subDIRectory

`rmdir` removes a subDIRectory in current directory -- note that a directory must be empty before it can be removed.

```
ooiwt@pe111:~$ rmdir tut01
ooiwt@pe111:~$ ls -F
ooiwt@pe111:~$ mkdir tut01
ooiwt@pe111:~$ ls -F
tut01/
```


## `cp`: CoPy files

```
ooiwt@pe111:~/tut01$ cp ~cs1010/tut01/hello.c .
ooiwt@pe111:~/tut01$ ls
hello.c
```
The command above copies the file `hello.c` from the HOME of user `cs1010`, under directory `tut01`, to the current directory.

If you want to copy the whole directory, use `-r` flag, where `r` stands for recursive copy.

```
ooiwt@pe111:~/tut01$ cp -r ~cs1010/tut01 .
```

In the last command above, the single `.` refers to the current directory.  

The directory `tut01` and everything under it will be copied to the current directory.

## `mv`: MoVe or rename files

`mv` can move files from one directory to another.

```bash
ooiwt@pe111:~/tut01$ ls
hello.c
ooiwt@pe111:~/tut01$ mv hello.c ..
ooiwt@pe111:~/tut01$ ls
ooiwt@pe111:~/tut01$ ls ..
hello.c
ooiwt@pe111:~/tut01$ mv ../hello.c .
```

Here, we tell `mv` to {--copy--} {++move++} a file `hello.c` from the parent directory to the current directory.

`mv` can also be used to rename files.

```
ooiwt@pe111:~/tut01$ mv hello.c hello_world.c
ooiwt@pe111:~/tut01$ ls
hello_world.c
```

!!! tip "Use TAB for Name Completion"
    If you have a very long file name, you may use UNIX's filename completion feature to reduce typing. For instance, you may type:
    ```
    ooiwt@pe111:~/tut01$ mv h
    ```
    and press the ++tab++ key, and UNIX will complete the filename for you if there is only one filename with the prefix "h". Otherwise, it will fill up the filename to the point where you need to type in more characters for disambiguation.
	The ++tab++ key can also complete the name of a command.

## `rm`: ReMove files

Be careful with this command -- files deleted cannot be restored.  There is no trash or recycled bin like in Mac or Windows.

```bash
ooiwt@pe111:~/tut01$ rm hello.c
ooiwt@pe111:~/tut01$ ls
ooiwt@pe111:~/tut01$
```

!!! warning "rm -rf *"
    While Unix command line provides lots of flexibility and power, with great power comes great responsibility.  Some commands are extremely dangerous.  `rm -rf *` is the most famous one.  The notation `*` refers to all files, and the flag `-f` means forceful deletion (no question asked!), and `-r` means remove recursively everything under the current directory tree.  Accidentally running this command has ruined many files.  [Read more here](https://www.quora.com/What-are-some-crazy-rm-rf-stories-you-have-heard-about)

`rm` comes with a `-i` flag that interactively asks you if you are sure if you want to delete a file.  It is a good idea to always run `rm -i`.  On `pe111`, we have configured everyone's account so that `rm` is aliased to `rm -i` by default.  So when you run `rm hello.c`, it actually runs `rm -i hello.c`.  

```bash
ooiwt@pe111:~/tut01$ rm hello.c
rm: remove regular file 'hello.c'?
```

Type `y` or `n` to answer yes or no respectively.

If you set up your own UNIX OS, you should add this alias

```bash
alias rm="rm -i"
```

to your `.bashrc` (Google to find out how).  Other useful aliases to avoid accidentally overwriting existing files are:

```bash
alias mv="mv -i"
alias cp="cp -i"
```

## `cat`: CATenate file content to the screen

To quickly take a look at the content of the file, use the `cat` command.

```bash
ooiwt@pe111:~/tut01$ cat hello.c
```

`less` is variant of `cat` that includes features to read each page leisurely.
```bash
ooiwt@pe111:~/tut01$ less hello.c
```

In `less`, use `<space>` to move down one page, `b` to move Back up one page, and `q` to Quit.

## `man`: Online MANual

An online help facility is available in UNIX via the `man` command (`man` stands for MANual). To look for more information about any UNIX command, for example, `ls`, type `man ls`. Type `man man` and refer to Man Pages to find out more about the facility. To exit `man`, press `q`.

Now that you are familiar with how the UNIX bash works, I won't show the command prompt any more in the rest of this article.

## `chmod`: Changing UNIX File Permission
It is important to guide our files properly on a multi-user system where users share the same file system.  UNIX has a simple mechanism to for ensuring that: every file and directory has nine bits of access permission, corresponds to three access operations, read (`r`), write (`w`), and execute (`x`), for four classes of users, the user who owns of the file (`u`), users in the same group as the owner (`g`), all other users (`o`), and all users (`a`) (union of all three classes before)

When you run `ls -l`, you will see the permission encoded as strings that look like `-rw-------` or `drwx--x--x` besides other file information.   

- The first character indicates if the file is a directory (`d`) or not (`-`).  
- The next three characters are the permission for the owner.  `rwx` means that the owner can do all three: reading, writing, and executing, `rw-` means that the owner can read and write, but cannot execute.
- The next three characters are the permission for the users in the same group.
- The last three characters are the permission for the users in the other groups.

To change permission, we use the `chmod` command.  Let's say that we want to remove the read and write permission from all other users in the group.  You can run:

```bash
chmod g-rw <file>
```

where `<file>` is the name of the file whose permission you want to change.  This would change the permission from `-rw-rw-rw-` to `-rw----rw-`, or from `-rwxr--r--` to `-rwx---r--`.

To add executable permission to everyone, you can run:

```bash
chmod a+x <file>
```

This would change the permission from `-rw-rw-rw-` to `-rwxrwxrwx`, or from `-rwxr--r--` to `rwxr-xr-x`, and so on.  You get the idea.

Another way to change the permission is to set the permission directly, instead of adding with `+` and removing with `-`.  To do this, one convenient way is to treat the permission for each class of user as a 3-bit binary number between 0 and 7.  So, `rwx` is 7, `rw-` is 6, `-w-` is 2, `---` is 0, etc.  

To set the permission of a file to `-r--r--r--` (readable by everyone), run:

```bash
chmod 444 <file>
```

To set the permission to `-rw-------`, run:

```bash
chmod 600 <file>
```

and so on.

It is important to ensure that your code is not readable and writable by other students, especially for graded lab exercises.

## `scp`: Secure Copy

Secure copy, or `scp`, is one way to transfer files from the programming environments to your local computer for archiving or storage.  Let's say you want to transfer a set of C files from the directory `a01` to your local computer, then, on your local computer, run:

```bash
ooiwt@macbook:~$ scp ooiwt@pe111:~/a01/*.c .
```

!!! warning
    If you have files with the same name in the remote directory, the files will be overwritten without warning.  I have lost my code a few times due to `scp`.  

The expression `*.c` is a _regular expression_ that means all files with filename ending with `.c`.  You can copy specific files as well.  For instance,

```bash
ooiwt@macbook:~$ scp ooiwt@pe111:~/a01/hello.c .
```


`scp` supports `-r` (recursive copy) as well.

## Specifying A Path in UNIX

In any command above, when we need to refer to a directory or a file, we need to specify an _unambiguous location_ of the directory or the file.  The most precise way to specify the location is to use the full path, or the _absolute path_.  For instance:

```bash
cp /home/o/ooiwt/tut01/hello.c /home/o/ooiwt/tut01/hello_world.c
```

That's a lot of characters to type.  We could shorten it in a few ways.  

- We could specify the location with respect to the home directory using `~`.  `~ooiwt` refers to the home directory of user `ooiwt`.  

```bash
cp ~ooiwt/tut01/hello.c ~ooiwt/tut01/hello_world.c
```

If you are `ooiwt`, then you can omit `ooiwt`, since `~` without any username refers to your home directory.

```bash
cp ~/tut01/hello.c ~/tut01/hello_world.c
```

- Or we could specify the location with respect to the current directory.  Suppose the current working directory is `~/tut01` (i.e., we have `cd` into `~/tut01`), then we could say this:

```bash
cp ./hello.c ./hello_world.c
```

Recall that a single dot `.` refers to the current directory.

The `./` however is redundant unless you are executing a command.  Since, by specifying a file name or a directory without a path (i.e., not using any `/`), the bash looks for the file or directory in the current directory.  So, we could just do:

```bash
cp hello.c hello_world.c
```

Another important short form for relative location is `..`.  Recall that this refers to the parent directory.  Suppose that the current directory is in `~/tut02`.  Then, to copy the files in `~/tut01`, you can run:

```bash
cp ../tut01/hello.c ../tut01/hello_world.c
```
