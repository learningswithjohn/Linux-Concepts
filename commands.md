Most important commands:

free -h
top
htop
vmstat
ps aux --sort=-%mem
cat /proc/meminfo
dmesg | grep -i oom
journalctl -k | grep -i oom
===========================================================================================================================================================
case - 1 : Users report the application is very slow.

Investigation:

You SSH into the server.

1. Check memory usage: free -h
2. Identify top memory consumers: ps aux --sort=-%mem | head
3. Watch the system: top
4. Check for OOM events: dmesg | grep -i oom
5. Review application logs: journalctl -u application.service
----------
Step 1: Assess the Situation

free -h
              total   used   free   shared  buff/cache  available
Mem:            16G    15.8G   150M      1G        5G        300M
Swap:            4G      4G      0

From the free -h output:
Available memory is very low (300 MB).
Swap is 100% utilized.
This can cause slowness because the kernel is constantly swapping pages between RAM and disk (thrashing).
At this point, I don't yet know the root cause.
------------
Step 2: Identify the Memory Consumer

ps aux --sort=-%mem | head    or    top

Questions to answer:

Which process is consuming memory? Is it Java? Is it Apache? Is it Tomcat? Is it Python?
--------------
Step 3: Check Whether the Kernel Killed Anything

dmesg | grep -i oom    or    journalctl -k | grep -i oom

If you see: Out of memory: Killed process 12345 (java)
You've found an important clue.
---------------
Step 4: Check the Application

Suppose it's Tomcat.

systemctl status tomcat

Check:

Is the service running?
Has it restarted recently?
Any recent failures?
-----------------
Step 5: Review Application Logs

tail -100 /opt/tomcat/logs/catalina.out

Look for:

OutOfMemoryError
Heap space errors
Connection pool issues
-------------------
Immediate Actions:

1. Restarting the application if it's safe and approved.
2. Scaling the application horizontally.
3. Increasing memory temporarily.
4. Configure monitoring and alerts.
5. Right-size the server or container limits.
===========================================================================================================================================================

Case -2 : when you run top or uptime command, what is load average means?

Load average shows how busy your system is. It counts how many programs are actively using the CPU or waiting for their turn.

If Load Average is: 0.40, 0.75, 1.20

These numbers represent the exponential moving average of system load over three specific time frames: Last 1 minute, Last 5 minutes, Last 15 minutes

On a 2 core CPU, if load average is : 1, 2, 2.5 
1: CPU has some capacity still
2: CPU is utilized 100% (since it is 2 core cpu and load average is 2...2/2=100% utilization)
2.5: CPU is overloaded and Processes are waiting.
===========================================================================================================================================================

Case -3 : top vs ps ....which one to use when?

Use top (or htop) when you need to:

1. Monitor live activity: Watch CPU and memory usage fluctuate second-by-second.
2. Identify resource hogs: Instantly spot which process is suddenly spiking your CPU or draining RAM right now.
3. Actively manage processes: Kill a process (by pressing k) or change its priority (r) without leaving the screen.
4. Track system health: Keep an eye on overall load average, uptime, and total memory usage in one continuous dashboard.

Use ps when you need to:

1. Find a specific process: Quickly check if a background service (like Docker or Nginx) is running (ps aux | grep nginx).
2. Write automation scripts: Parse process IDs (PIDs) using tools like awk or grep inside Bash scripts.
3. Inspect specific attributes: View hidden details like the exact command line path used to start an app, parent-child relationships (ps -ef), or custom columns (ps -eo pid,ppid,cmd).
===========================================================================================================================================================




