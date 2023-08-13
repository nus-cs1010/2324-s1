# The CS1010 Programming Environment

C is a common programming language.  You can find different implementations of a C compiler on many platforms.  Wikipedia [lists more than 40 different C compilers](https://en.wikipedia.org/wiki/List_of_compilers#C_compilers).  These different compilers support different processor architectures and operating systems and may behave slightly differently as well as support different features of C standards.  It is therefore important for CS1010 to stick to a single platform and single compiler.

Our platform of choice is **Ubuntu 20.04.** using the `clang` compiler (**version 10.0 or later**).

The school has provided a list of computing servers for you to use.  You can access them remotely via `ssh` (Secure SHell).  The hosts are named `pe111`, `pe112`, ... , `pe120`.  (`pe` stands for "programming environment").  We will refer to these servers generally as the _PE hosts._

!!! note "pe115 and pe116"
    For this semester, the two servers `pe115` and `pe116` are not available.

You can choose which of the 8 hosts to use.  You share the same home directory across all the hosts (this home directory, however, is different from that of `stu`).

For simplicity, the following guide uses `pe111` in all examples.  Do keep in mind that you can use other hosts for CS1010 to spread out the load.

While you could complete your programming assignments on your own computers, the practical exams are done in a controlled environment using servers similar to the PE hosts.  It is therefore advisable for you to familiarize yourself with accessing the PE servers via `ssh` and edit your program with either `vim`.

## Basic Requirements

1. You need to have an SoC Unix account.  If you do not have one, you can [apply for one online](https://mysoc.nus.edu.sg/~newacct/).

2. Once you have an account, you need to [activate your access to the PE hosts](https://mysoc.nus.edu.sg/~myacct/services.cgi), which are part of the SoC computer clusters.

3. You need a command line `ssh` client.  Windows 10/11, macOS, and Linux users should already have `ssh` installed by default.  If your OS does come with `ssh` (i.e., it cannot find the `ssh` command when you type `ssh` into your terminal), look for instructions on how to install OpenSSH client on your operating system.

4. You need a [terminal emulator](unix-background.md#what-is-a-terminal).  The default terminal emulator that comes with Windows and Mac supports only basic features.  For Windows 10/11 users, CS1010 recommends either PowerShell (pre-installed) or [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/).  For macOS users, CS1010 recommends [iTerm2](https://iterm2.com/).

For older versions of Windows, you can check out [XShell 6](https://www.netsarang.com/products/xsh_overview.html) (free for home/school use), or [PuTTY](https://www.putty.org).  These are GUI-based programs so the command lines instructions below do not apply.

## The Command to SSH

To connect to a remote host, run the following in your terminal on your local computer:
```
ssh <username>@<hostname>
```

Replace `<username>` with your SoC Unix username and `<hostname>` with the name of the host you want to connect to. For instance, I would do:
```
ssh ooiwt@pe112.comp.nus.edu.sg
```

if I want to connect to `pe112.comp.nus.edu.sg`.

After the command above, follow the instructions on the screen.  The first time you ever connect to `pe112.comp.nus.edu.sg`, you will be warned that you are connecting to a previously unknown host.  Answer `yes`.  After that, you will be prompted with your SoC Unix password.  Note that nothing is shown on the screen when your password is being entered.

<script id="asciicast-4rtH1KENV6QOdKtlY0T7mce0M" src="https://asciinema.org/a/4rtH1KENV6QOdKtlY0T7mce0M.js" async></script>

## Accessing The PE Hosts from Outside SoC

The PE hosts can only be accessed from within the School of Computing networks.  If you want to access it from outside, you need to connect through SoC VPN.

First, you need to set up a Virtual Private Network (VPN) (See [instructions here](https://dochub.comp.nus.edu.sg/cf/guides/network/vpn)).  The staff at `helpdesk@comp.nus.edu.sg` or the IT helpdesk in COM1, Level 1, will be able to help you with setting up if needed.

!!! note "SoC VPN vs NUS VPN"

    Note that SoC VPN is different from NUS VPN.  Connecting to NUS VPN only allows you access to the NUS internal network, but not the SoC internal network.

## Copying Files between PE Nodes and Local Computer

Secure copy, or `scp`, is one way to transfer files between the programming environments and your local computer.   `scp` behaves just like `cp` (see [Unix: Essentials](unix-essentials.md)).  The command takes in two arguments, the source and the destination.   The difference is that we use the `<username>@<hostname>:<filename>` notation to specify a file on a remote host.

Let's say you want to transfer a set of C files from the directory `a01` to your local computer, then, on your local computer.  You run:

```bash
ooiwt@macbook:~$ scp ooiwt@pe111.comp.nus.edu.sg:~/a01/*.c .
```

!!! warning
    If you have files with the same name in the remote directory, the files will be overwritten without warning.  I have lost my code a few times due to `scp`.  

The expression `*.c` is a regular expression that means all files with filename ending with `.c` (see [Advanced Topics on Unix](unix-advanced.md)).
You can copy specific files as well.  For instance, to copy the file `hello.c` from your local computer to your `~/a01` directory:

```bash
ooiwt@macbook:~$ scp hello.c ooiwt@pe111.comp.nus.edu.sg:~/a01
```

`scp` supports `-r` (recursive copy) as well.

Note that we always run `scp` on your local computer in the examples above, since the SSH server runs on the PE node.

## Setting up SSH Keys

The next step is not required but is a time-saver and a huge quality-of-life improvement.  You need to be familiar with basic Unix commands, including how to copy files to remote hosts (using `scp`) and how to check/change file permissions (using `ls -l` and `chmod`).  If you are still not comfortable with these commands, make sure you play with the [basic Unix commands](unix-essentials.md) first.  You can come back and complete this step later. 

Our goal here is to set up a pair of public/private keys for authentication so that you do not need to type your password every time you log into a PE host.

You can use the following command on your local computer to generate a pair of keys:
```
ssh-keygen -t rsa
```

This command will generate two keys, a private key `id_rsa` and a public key `id_rsa.pub`.  Keep the private key `id_rsa` on your local machine in the hidden `~/.ssh` directory, and [copy](environments.md#copying-files-between-pe-nodes-and-local-computer) the public key `id_rsa.pub` to your home directory on PE `pe111`.  On `pe111`, run
```
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

Make sure that the permission for `.ssh` both on the local machine and on PE is set to `700` and the files `id_rsa` on the local machine and `authorized_keys` on the remote machine are set to `600`.  Once set up, you need not enter your password every time you run `ssh` or `scp`.  

!!! Tips "Using ssh-copy-id"
    If you have [`ssh-copy-id`](https://www.ssh.com/academy/ssh/copy-id) installed (e.g., this is pre-installed with macOS), you can simplify the above to two steps:
    ```
    ssh-keygen -t rsa
    ssh-copy-id <username>@pe111.comp.nus.edu.sg
    ```

## Stability of Network Connection
    
Note that a stable network connection is required to use the PE hosts for a long period without interruption.   If you encounter frequent disconnections while working at home or on campus while connected wirelessly, please make sure that your Wi-Fi signal is strong and that there is no interference from other sources. 

If your connection is disconnected in the middle of editing, `vim` saves the state of the buffer for you.  See the section on [recovery file](vim-tips.md#5-recovery-files) on how to recover your files.

If you experience frequent disconnection, you can consider running `screen`. After logging into a PE host, run:

```
screen
```

You will see some messages, press ++Enter++ to go to the command prompt. You can now use the PE host as usual. In case you are disconnected (e.g., in the middle of editing), you can log into the same PE host again, and run:

```
screen -r
```

to resume your previous session.


## Troubleshooting

Suppose you try to connect to `pe111` using:
```
ssh ooiwt@pe111.comp.nus.edu.sg
```
and you get the following error:

1. `ssh: Could not resolve hostname pe111.comp.nus.edu.sg`

	`ssh` cannot recognize the name `pe111`, it is likely that you are not connected to the SoC VPN.

2. `Connection closed by 192.168.48.xxx port 22`

    You have connected to the PE host, but you are kicked out because you have no permission to use the host.

	Make sure you have activated your access to "SoC computer clusters" here: https://mysoc.nus.edu.sg/~myacct/services.cgi

3. `Permission denied, please try again`

    You did not enter the correct password or username.  Please use the username and password 
of your SoC Unix account which you have created here: https://mysoc.nus.edu.sg/~newacct/.

    Check that you have entered your username correctly.  It is _case-sensitive_.

    If you have lost your password, go here: https://mysoc.nus.edu.sg/~myacct/resetpass.cgi

4. `Could not chdir to home directory /home/o/ooiwt: Permission denied`

    This error means that you have successfully connected to the PE hosts, but you have no access to your home directory. 

	This should not happen.  Please send an email with the above error message to helpdesk@comp.nus.edu.sg, including the PE hosts that you
	connected to with this error and your username.  The system administrator can reset the permission of your home directory for you.
