Linux Architecture

Whenever an interviewer asks - Can you explain Linux architecture? ......
Many candidates immediately start talking about the kernel. Interviewers usually want to know whether you understand how all the components interact.

Imagine you run: ls -lrt
Have you ever wondered what actually happens?

There are several layers involved.

+-------------------------------------+
|             User                    |
+-------------------------------------+
                |
                v
+-------------------------------------+
|             Shell                   |
|      (bash / zsh / sh)              |
+-------------------------------------+
                |
                v
+-------------------------------------+
|        System Call Interface        |
+-------------------------------------+
                |
                v
+-------------------------------------+
|             Kernel                  |
| Process | Memory | Network | FS     |
| Device Drivers | Security           |
+-------------------------------------+
                |
                v
+-------------------------------------+
|             Hardware                |
| CPU | RAM | Disk | NIC              |
+-------------------------------------+


Layer 1 — Hardware

This is the physical machine. Examples: CPU, RAM, SSD/HDD, Keyboard, Network card, GPU etc.
Hardware does nothing on its own. It needs software to manage it.
===================================================================================================================================================================

Layer 2 — Kernel

The kernel is the heart of Linux. The kernel is the core, low-level software component of an operating system (OS) that acts as the primary interface between computer hardware and user-space applications.
Kernel decides - 
1. Which process gets CPU time
2. Which process gets memory
3. Which process can access a file
4. Which packet goes over the network
5. Which driver talks to the hardware

Responsibilities of the Kernel
Process Management

Example:

You run: python app.py

The kernel:

Creates a process
Assigns a PID
Schedules CPU time
Cleans up resources when it exits
===================================================================================================================================================================

Layer 3 - System Libraries

Applications don't usually talk to the kernel directly. Instead, they use libraries.

Ex: printf("Hello");

Eventually, the C library (glibc) invokes a system call to ask the kernel to perform the requested operation. This abstraction makes application development much easier.
===================================================================================================================================================================

Layer 4 - Shell

The shell is the command interpreter and it reads your commands, parses them, start programs, handles variables, pipes and redirection.

Ex: bash, sh, zsh, ksh

when you run : ls -lrt, the shell does not execute itself. It locates the executable path (where ls is defined), creates a new process, and asks the kernel to run it.
===================================================================================================================================================================

Layer 5 - User Applications

Applications like Tomcat, Docker, Kubernetes, Python, MySQL all rely on the Operating system services provided by the lower layers.
===================================================================================================================================================================

What happens when you run : ls -lrt

1. The shell reads the command.
2. It searches the directories listed in $PATH to find the ls executable.
3. It asks the kernel to create a new process.
4. The kernel loads the executable into memory.
5. The ls program requests the directory information through system calls.
6. The kernel reads the directory entries and metadata from the filesystem.
7. the kernel returns the data to ls.
8. ls formats the output.
9. The shell displays the output on your terminal.























