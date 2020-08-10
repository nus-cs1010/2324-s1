# The CS1010 Programming Environment

C is a common programming language and you can find different implementations of C compiler on many platforms.  Wikipedia [lists more than 40 different C compilers](https://en.wikipedia.org/wiki/List_of_compilers#C_compilers).  These different compilers support different processor architecture, operating systems, may behave slightly differently, and may support different features of C standards.  It is therefore important for CS1010 to stick to a single platform and single compiler.

Our platform of choice is **Ubuntu 16.04 (or later)** using the `clang` compiler (**version 3.4.2 or later**).

The school has provided a list of computing servers for you to use.  You can access them remotely via `ssh`, or secure shell.  The hosts are named `pe111`, `pe112`, ..., `pe120`.  (`pe` stands for "programming environment").

You can choose which of the 10 hosts to use.  You share the same home directory across all 10 hosts (this home directory, however, is different from that of `sunfire`).

For simplicity, the following guide uses `pe111` in all examples.  Do keep in mind that you can use other hosts for CS1010 to spread out the load.

While it is possible for you to complete the programming assignments on your own computers, the practical exams are done in a controlled environment using the desktop PCs in the programming labs.  It is therefore advisable for you to familiarize with accessing remote computing servers via `ssh` and edit your program with either `vim` or `macro`.

## Basic Requirements

1. You need to have an SoC UNIX account.  If you do not have one, you can [apply for one online](https://mysoc.nus.edu.sg/~newacct/).

2. Once you have an account, you need to [activate your access to the PE hosts] (https://mysoc.nus.edu.sg/~myacct/services.cgi), which is part of the SoC computer clusters.

3. You need an `ssh` client.  macOS and Linux users should already have it installed by default.  Windows 10 users should install [the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and use Linux.

For older versions of Windows, such as those used in the SoC's programming labs, you can check out [XShell 6](https://www.netsarang.com/products/xsh_overview.html) (free for home/school use), or [PuTTY](https://www.putty.org).  These are GUI-based programs so the command lines instructions below do not apply.

## The Command to SSH

Run:
```
ssh <username>@pe111.comp.nus.edu.sg
```

Replace `<username>` with your SoC UNIX username, for instance, I would do:
```
ssh ooiwt@pe111.comp.nus.edu.sg
```

After the command above, following the instructions on the screen.  The first time you ever connect to `pe111.comp.nus.edu.sg`, you will be warned that you are connecting to a previously unknown host.  Say `yes`, and you will be prompted with your SoC UNIX password.

## Accessing The PE Hosts from Outside SoC

The PE hosts can only be accessed from within the School of Computing networks.  If you want to access it from outside, there are two ways.

### Using SoC VPN
One way is to set up a Virtual Private Network (VPN) (See [instruction here](https://dochub.comp.nus.edu.sg/cf/guides/network/vpn)).  The staff at `helpdesk@comp.nus.edu.sg` or the IT helpdesk in COM1, Level 1, will be able to help with you setting up if needed.

!!! note "SoC VPN vs NUS VPN"

    Note that SoC VPN is different from NUS VPN.  Connecting to NUS VPN only allows you access to the NUS internal network, but not the SoC internal network.

### Tunneling through Sunfire

The alternative is to use ssh tunnels.

SoC's Sunfire (`sunfire.comp.nus.edu.sg`) is configured to allow your connection if it's originating from a local telco. (See [more details here](https://docs.comp.nus.edu.sg/node/1824).)  Since `sunfire` is situated within the School of Computing network, `sunfire` is able to access the VM.  This opens the possibility of connecting from your device (using an Internet connection from a local telco) to `sunfire`, and then from `sunfire` to the VM.

There are two ways to achieve this, and in both ways, it appears to the PE hosts that Sunfire is the client.

#### SSH Using Sunfire's Terminal

Connect to Sunfire at `sunfire.comp.nus.edu.sg` via your favorite SSH client.  After logging in, run the command `ssh pe111` to connect to the programming environment.  This effectively starts an SSH session to the programming environment from within your existing SSH session to Sunfire.  Refer to instructions below on how to connect via SSH.

#### SSH Port Forwarding

SSH has built-in support for local and remote port forwarding, and local port forwarding can be used to connect to the programming environments.  Local port forwarding means that a port of the SSH client (your machine) is forwarded to the SSH server (`sunfire`), which opens a connection to a preset destination server (e.g., `pe111`).  This method causes the host to seem as if it is hosted on a local port, e.g. `localhost:1010`, allowing you to use your favorite SCP program (e.g. [FileZilla](https://filezilla-project.org/)) to access the PE host.

To use local port forwarding (from local port `1010`), connect to `sunfire` using
```
ssh -L 1010:pe111.comp.nus.edu.sg:22 <username>@sunfire.comp.nus.edu.sg
```
This command opens an SSH tunnel from port `1010` of your machine to port `22` (the default SSH port) of `pe111.comp.nus.edu.sg` via `sunfire`.  After successful login, open a separate SSH (or SCP) connection from your machine to `localhost` at port `1010` to access the PE host:

```
ssh <username>@localhost -p 1000
```

[`PuTTY`](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) supports SSH port forwarding, so this setup can also be used on Windows.

## Setting up SSH Keys

Once you are comfortable with UNIX, you can set up a pair of public/private keys for authentication.  

You can use
```
ssh-keygen -t rsa
```

to generate a pair of keys on your local computer.  Keep the private key `id_rsa` on your local machine in the hidden `~/.ssh` directory, and copy the public key `id_rsa.pub` to your home directory on PE `pe111`.  On `pe111`, run
```
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

Make sure that the permission for `.ssh` both on the local machine and on PE are set to `700` and the files `id_rsa` on the local machine and `authorized_keys` on the remote machine are set to `600`.  Once set up, you need not enter your password every time you run `ssh` or `scp`.  
