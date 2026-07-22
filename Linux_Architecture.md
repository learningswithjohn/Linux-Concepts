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
Memory Management

Suppose:

Chrome requests 2 GB
Tomcat requests 8 GB
MySQL requests 4 GB

The kernel decides:

Where to allocate memory
Whether to use swap
Which pages to reclaim
Whether the OOM Killer must terminate a process

Interview question:

What is OOMKilled?

You should know that it's the kernel terminating a process because the system ran out of memory.
===================================================================================================================================================================
File System Management

When you run: cat test.txt

The kernel:

Checks permissions
Finds the inode
Reads blocks from disk
Returns the contents to the application
===================================================================================================================================================================
Networking

When a browser opens: https://google.com

The kernel:

Creates a socket
Handles TCP/IP
Sends packets
Receives packets
Closes the connection
===================================================================================================================================================================
Device Drivers

Suppose you insert a USB drive.

The kernel:

Detects it
Loads the correct driver
Makes it available to the operating system
===================================================================================================================================================================

















