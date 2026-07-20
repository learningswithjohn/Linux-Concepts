
Linux namespaces and cgroups are the two core linux kernel technologies that make containers like docker possible.
========================================================================================================================================================================
Namepsaces - isolation

Namespaces isolates system resources so that a process thinks it has its own private environment. A process running inside a namespace believes it is the only process on the machine with access to those specific resources.

Below are are several namespace types:

PID - which isolates process IDs. Container sees its own PID 1 but not host processes. Inside the namespace, a containerized app thinks it is PID 1 (the main init process), but on the host system, it might actually be PID 4502.

NET (Network) - Isolates network devices, IP addresses, and routing tables. A container gets its own virtual network card (eth0) and loopback interface (lo).

MNT (mount points) - container has its filesystem view. Isolates file system mount points. A process inside this namespace sees a completely different file system structure (like a mini-root directory) than the host.

USER - Isolates user and group IDs. This is huge for security. You can be the all-powerful root (User ID 0) inside your container, but on the actual host machine, you are just a standard, unprivileged user.

CGROUP - While namespaces hide things, cgroups restrict resource usage. Without cgroups, a single runaway container could crash your entire host machine by eating up 100% of the CPU or RAM.
========================================================================================================================================================================
C groups or Control groups - which restricts resource usage

While namespaces hide things, cgroups restrict resource usage. Without cgroups, a single runaway container could crash your entire host machine by eating up 100% of the CPU or RAM.

Cgroups control resources using components called subsystems or controllers:

CPU: Limits how much processor time a group of processes can use (e.g., "This container gets a maximum of 2 CPU cores").

Memory: Limits RAM usage. If a process exceeds its allowed memory limit, the cgroup triggers the Out-Of-Memory (OOM) killer to terminate it before it crashes the server.

BlkIO (Block I/O): Restricts disk read and write speeds. This stops one application from hogging all the hard drive performance.
========================================================================================================================================================================

How namespaces and C groups work together?

When you run a command like docker run -d -m 500m nginx, the Linux kernel instantly goes to work behind the scenes:

1. It uses namespaces to give Nginx its own network, fake root folder, and isolated process ID.
2. It attaches a cgroup to that Nginx process, configuring the kernel to kill it if it tries to use more than 500 megabytes of memory.

Real world example:

Imagine you have 2 containers:

Container A: JAVA application
Container B: MySQL Database

Namespaces ensure: 
1. Java cannot see MySQL's processes.
2. Each has its own network and hostname.

C groups ensure:
1. Java gets 2 CPUs and 4 GB RAM
2. MySQL gets 4 CPUs and 8 GB RAM

Neither container can consume all the server's resources.
========================================================================================================================================================================
Short explanation - 

Linux namepsaces provide isolation by giving processes their own view of system resources, while C groups provide resource control by limiting and accounting for CPU, Memory, I/O and other resources.

Containers like docker rely on both namespaces and cgroups to provide secure, isolated and resource-managed environments.


