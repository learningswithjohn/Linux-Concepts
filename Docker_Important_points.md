Docker important points
===========================================

A container is a bundle of Application, Application libraries required to run your application and the minimum system dependencies.

<img width="888" height="557" alt="image" src="https://github.com/user-attachments/assets/89d6f3d3-1de9-4283-846c-85a66f62a021" />

Hypervisor - A hypervisor is a software layer (or firmware/hardware in some cases) that allows multiple virtual machines (VMs) to run on a single physical server.
Think of it as a manager sitting between the physical hardware and the virtual machines.

Hypervisor main work is to distribute physical server resources(like CPU, Memory, storage, network etc) dynamically among active virtual machines.

Hypervisor products : VMware ESXi, Microsoft Hyper-V, and Linux KVM etc.

To provide a better picture of files and folders that containers base images have and files and folders that containers use from host operating system, 
Please refer below:

Files and Folders in containers base images:

    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.

Files and Folders that containers use from host operating system:

    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
================================================================================================================================================================

Case 1 : Consider we have 5 containers. we know that containers share host OS and kernel....
In that case what will be the situation of other containers if 1 containers tries to consume majority of the host resources?

Solution: If one container tries to consume the majority of the host resources, it will starve the other 4 containers of CPU and memory, 
potentially slowing them down or crashing them entirely. Because containers share the same host kernel and OS, they directly compete for the same pool of physical hardware unless strict boundaries are enforced.

Here is exactly what happens to other 4 containers

<img width="855" height="566" alt="image" src="https://github.com/user-attachments/assets/5a9ba943-78eb-4f9a-a197-c0982a6e01ae" />

Here is the exact solution to prevent a single container consuming majority of host resources.

<img width="867" height="537" alt="image" src="https://github.com/user-attachments/assets/872c9a45-ef65-4ac9-a640-36e76c9d07b8" />

================================================================================================================================================================


