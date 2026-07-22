Process management:

A process is simply a running instance of a program. 

Ex: firefox (on disk) = program
    Running firefox = process

Every Linux process contains specific metadata and resources managed by the kernel:

PID (Process ID): A unique numerical ID assigned to every process.
PPID (Parent Process ID): The ID of the process that created it.
Memory Space: Private RAM containing the program code, variables, and stack.
Security Credentials: The specific user and group ownership that dictates what files the process can access.
File Descriptors: A list of open files, network sockets, and input/output streams used by the process.

The Process Lifecycle:
--------------------------

Processes are organized in a strict parent-child hierarchy.

1. Creation: An existing parent process duplicates itself using the fork() system call, then loads a new program using exec().
2. Execution: The process cycles through states like Running (using the CPU) or Sleeping (waiting for data or user input).
3. Termination: When finished, the process exits and returns a status code to its parent. If a parent dies before its child, the child becomes an Orphan and is adopted by the system's root init process (systemd). If a child finishes but its parent fails to read its exit status, it becomes a Zombie process.

Every process typically goes through these stages:

New -> Ready -> Running -> Waiting (I/O) -> Running -> Terminated

===================================================================================================================================

When you run : ps -ef  (or)  ps aux

you will see process states such as :

1. R - Running or runnable
2. S - Interruptible sleep
3. D - Uninterruptible sleep (usually waiting for disk I/O)
4. T - Stopped
5. Z - Zombie
===================================================================================================================================

Important Process management commands:

1. Viewing Processes (ps):

   The ps command takes a snapshot of your system.
   List all running processes: ps aux
   Find a specific process by name: ps aux | grep nginx

2. Real-Time Monitoring (top / htop)

   These tools update dynamically to show you what is consuming CPU and RAM.
   Standard monitor: Type 'top' in your terminal. Press q to exit.
   Modern monitor: Type htop (if installed) for a color-coded, scrollable interface where you can kill processes directly using the F9 key.

3. Stopping Unresponsive Applications (kill)

   If a program freezes, you can force it to close using its PID found via ps or top.
   Graceful termination (preferred): kill 1234 (Sends SIGTERM, letting the app save data and close cleanly)
   Forceful termination (emergency): kill -9 1234 (Sends SIGKILL, immediately killing the process)
   Kill by name: killall firefox (Closes all instances of a program at once)

Note : pgrep (Process grep)
What it does: Directly queries the Linux kernel's process list to find running programs matching your search term.
How it works: It does not need ps or pipes (|). It searches natively and, by default, returns only the Process IDs (PIDs).
Ex: pgrep firefox
Count the processes: pgrep -c nginx (Returns just the number of running instances).

===================================================================================================================================

1. Program vs. Process

   Program: A passive entity. It is the static code, instructions, and data compiled into an executable file sitting silently on your hard drive (e.g., /usr/bin/firefox).
   Process: An active entity. It is created the exact moment a program is loaded into RAM and executed by the CPU. A process is a living program in action, complete with its own memory space, PID, and system resources.

2. What is a Thread?

   A Segment of a Process: If a process is a house, threads are the people working inside it. A process is a container for resources, while a thread is the actual unit of execution that the CPU schedules.
   Shared Memory: Multiple threads created inside the same process share that process’s memory space, open files, and environment.
   The Benefit: Because they share memory, threads can talk to each other much faster than completely separate processes can. For example, a web browser process might use one thread to download an image, another thread to render text, and a third thread to listen to your mouse clicks.

3. Orphan Processes

   The Definition: An orphan is a running child process whose parent process has terminated (died) or crashed unexpectedly.
   The Savior (systemd): In Linux, processes cannot exist without a parent. When a process becomes an orphan, it is immediately adopted by the root system process (usually systemd with PID 1).
   The Result: systemd takes over, monitors the orphan, and cleans up its resources when it finally finishes executing. Orphan processes generally do not harm system performance.

4. Zombie Processes

   The Definition: A zombie is a dead process. It has completely finished its execution, freed its memory, and stopped running, but it still occupies a slot in the system's process table.
   The Cause: When a child process dies, it leaves behind an exit status code for its parent. A zombie occurs when the parent process is too busy or poorly programmed to read this exit status (via the wait() system call).
   The Danger: Zombies do not consume CPU or RAM, but they do consume a PID slot. If a buggy program creates thousands of zombies, it can exhaust the system's maximum PID limit, preventing you from launching any new applications. You cannot kill a zombie with kill -9 because it is already dead; you must kill or restart its parent process to clear it.


Summary Checklist:

Program: Code on disk.
Process: Code running in RAM.
Thread: A mini-worker inside a process.
Orphan: Alive child, dead parent (Adopted by systemd).
Zombie: Dead child, unresponsive parent (Stuck in the process table).
=================================================================================================================================

The Virtual Filesystem (/proc):

In Linux, "everything is a file." The kernel exposes real-time process data inside the /proc directory. Every running PID has its own folder here.

ls /proc/<PID>: Explore the directory of a running process.

/proc/<PID>/cmdline: The exact command that started the process.

/proc/<PID>/status: Human-readable status (memory usage, parent PID, threads).

lsof (List Open Files): See exactly what files or network ports a process is using.
lsof -i :80 (Finds which process is occupying web port 80).

pstree: Displays running processes as a visual tree showing parent-child relationships clearly.

strace: Traces system calls made by a process. Incredible for debugging why an application is freezing or failing to open a file.
strace -p <PID>
=================================================================================================================================

Linux SIGNALS:

You know kill and kill -9, but you should learn how processes handle different underlying kernel signals:

SIGTERM (15): The polite request to stop. Applications can catch this signal, flush data to disk, close network connections, and exit safely.

SIGKILL (9): The un-catchable kill switch. The kernel immediately deletes the process from RAM. The application cannot clean up after itself, which can lead to file corruption.

SIGHUP (1): Hangup. Used by many daemons (like Nginx or Apache) to reload their configuration files smoothly without restarting the entire process.
=================================================================================================================================




