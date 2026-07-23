Memory management:


Linux memory management is the subsystem responsible for allocating, tracking, protecting, and reclaiming memory used by processes and the kernel. Its main goal is to maximize performance while efficiently utilizing available RAM.

Every process in Linux does not interact with your actual RAM chips(Physical memory) directly. Instead, it interacts with Virtual Memory.
=====================================================================================================================================
Virtual Memory: Linux uses virtual memory, which gives each process its own isolated address space.

Benifits:
Process isolation and security
Efficient memory utilization
Ability to use more memory than physically available through swap

Physical Memory: Physical memory refers to the actual RAM installed on the system.

Linux divides physical memory into fixed-size pages. Common page sizes:
      4 KB (most common)
      2 MB (Huge Pages)
      1 GB (Gigantic Pages)

Pages: Memory is chopped up into small, fixed-size blocks called pages. On almost all standard Linux systems, a single page is exactly 4 KB (4096 bytes).

Paging: Paging is a memory management scheme that eliminates the need for physical memory to be contiguous. It allows the operating system to break memory into small, fixed-size blocks so it can allocate RAM efficiently without wasting space.

Virtual Pages (Process Side): The kernel divides a process's virtual memory into fixed-size blocks called Pages.

Physical Frames (RAM Side): The kernel divides the actual physical RAM into blocks of the exact same size, called Page Frames (or simply frames).

The Standard Size: On almost all modern x86 Linux architectures, a standard page/frame size is exactly 4 KB (4,096 bytes).

Because pages and frames are identical in size, one virtual page maps precisely to one physical frame.
=====================================================================================================================================

Swap space:

Swap space is a designated area on a storage drive (such as an SSD or HDD) that the Linux kernel uses as emergency overflow memory when the system’s physical RAM becomes completely full.

When RAM becomes insufficient, Linux can move inactive pages to disk storage called swap.

Advantages:
1. Prevents immediate out-of-memory conditions
2. Supports memory overcommit

Disadvantages:
1. Much slower than RAM
2. Excessive swapping causes performance issues (thrashing)

We can check swap space using the 'free -h' command and look fpor the second line in the below output:

               total        used        free      shared  buff/cache   available
Mem:            15Gi       8.2Gi       2.1Gi       1.1Gi       5.2Gi       6.1Gi
Swap:          2.0Gi       512Mi       1.5Gi
=====================================================================================================================================




