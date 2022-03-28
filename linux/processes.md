# Managing Running Processes:

## Table of Content:
* [Table of Content](#table-of-content)
* [Intro](#intro)
* [Processes](#processes)
* [Listing Processes](#listing-processes)
	+ [Listing Processes with`ps`](#listing-processes-withps)
	+ [Listing and Changing Processes with `top`](#listing-and-changing-processes-with-top)
* [Managing Foreground and Background Processes](#managing-foreground-and-background-processes)
* [Killing and Renicing Processes](#killing-and-renicing-processes)
	+ [Killing Processes with `kill` and `killall`](#killing-processes-with-kill-and-killall)
		+ [Using `kill` to Signal Processes by PID](#using-kill-to-signal-processes-by-pid)
		+ [Using `killall` to Signal Processes by Name](#using-killall-to-signal-processes-by-name)
	+ [Setting Process Priority with `nice` and `renice`](#setting-process-priority-with-nice-and-renice)
## Intro:
- Linux inherits from Unix the fact that it is a **multi-tasking** system, meaning that multiple **processes** can run at the same time. A process is an instance of a running program. Linux gives you tools that allow to:
	- Listing running processes.
	- Monitoring system usage by these processes.
	- Pausing processes.
	- Killing processes. 
	- Putting processes in the background or brining them to the foreground.
- This document will go over a few of these process management tools such as `ps`,  `top`, `kill`, `jobs`, and others. 

## Processes:
- *A process is an instance of a running program* might be vague. A better way to explain it is that a system would have only one `vi` command (program), if 15 users are using your system simultaneously and all running `vi`, then there are 15 `vi` processes running.
- A process is identified by its **process id** (commonly abbreviated to *PID*). This is unique to each process and no two processes can share the same PID. When the process is killed, that number can be reused and assigned to another process. 
- In addition to the PID, a process has other attributes such as the user or group it runs under. Such information tells us what the process can and cannot do and what resources it has access to. A process running as root has much more access (in fact total access) than a process running under a regular user. 
- Managing processes is a crucial skill for Linux system administrators as out-of-control processes can decrease or kill the performance of a system. This document will go into finding and handling processes based on their usage of CPU, memory and other resources.
- Commands that list and show information	about processes is stored in the **`/proc`** directory. A process stores its information in `/proc` under a subdirectory named after the process's PID. I checked this and it has a lot of information.  

## Listing Processes:
### Listing Processes with`ps`:
- **`ps`** is for processes something like `ls` is for files. It. tells you which files are running, who is running them and the resources they are using:
```sh
ps u
```
- By running the command above, you'd get an output similar to the following:
```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
am          1196  0.0  0.1   8928  5928 pts/0    Ss   Mar26   0:00 -bash
am          4173  0.0  0.0   8888  3248 pts/0    R+   10:01   0:00 ps u
```
- Running `ps` by itself gets less information, but adding `u` option gives you much more detail. Let's try. to decode this output. This output shows two processes:
	1. Process with PID 1196 shows that user `am` opened a bash shell after logging in.
	2. User `am` started a process (PID: 4173) of the `ps u` command.
- These two processes are associated with the `pts/1` terminal?!! (*I am using ssh to connect to the system. In a local system I see `tty1`*).
- Let's see what the columns in the output from `ps u` mean:
	- **`USER`** is for the username which is `am` here.
	- **`PID`** is the PID of the process. You can kill or send a system call to a process using the PID.
	- **`%CPU`** is for the percentage of CPU resources consumed by the process.
	- **`%MEM`** is for the percentage of memory resources the process consumes.
	- **`VSZ`** stands for **virtual set size** and it shows the memory (in kilobytes) allocated to the process.
	- **`RSS`** (**residnt set size**) means the actual amount of memory (in kilobytes) being used by the process. 
	- **`TTY`** refers to how/terminal you use to connect to the system. 
	- **`STAT`** represents the state of the process. `R` is for running processes, while `S` is for sleeping ones. Something like `R+` means the process is in the foreground. 
	- **`START`** refers to when the process started running.
	- **`TIME`** refers to the accumulative time since the process started. It's usually `00:00` for most programs because most use very little CPU time. 
	- **`COMMAND`** simply refers to the program name. 
- There are other processes running in a system that are not associated with the terminal. These are background processes that listen to IO, manage audio and windows (in desktop environments), networking, etc. Some of these might start when the system boots and keep running until the system shuts. To see these processes, you'd need the **`x`** option as in:
```sh
ps ux | less
```
- We pipe the output to `less` to make paging through it easier. 
- With the **`a`** option, you can list processes for all users including root:
```sh
ps aux
```
- The **`e`** option allows you list every option, but it works like plain `ps` and doesn't show other columns. You can combine it with the **`o`** option to get specific columns from every process as the following example shows:
```sh
ps -eo pid,user,uid,group,gid,vsz,rss,comm 
```
- As you can see, there are other information that are not shown by default such as `gid` for group id and `uid` for user id.
- We can also sort processes based on some attribute like memory or CPU usage with the **`--sort`** option:
```sh
ps -eo comm,pid,rss --sort=-rss | head -n 20
```
- The example above sorts processes by their `rss`. The hyphen preceding the `rss` means sort them in reverse to see which processes are using the highest amount of memory.

### Listing and Changing Processes with `top`: 
- Another cool program for listing running processes is **`top`**. In addition to listing running processes, `top` also allows you to kill and *renice (reprioritize)* processes. It lists processes and other system information in a screen-oriented way, something similar to how `less` shows things as opposed to `cat`. It basically lists all running processes and sorts them by CPU usage by default, but you can change what column to sort things by.
- You need to run as root to kill/renice some processes, but you can also kill/renice your own processes without root privileges. 
- At the top of the `top` is some general information about the system as a whole:
	- **`up`**: How long the system has been up.
	- **`<number> user`**: How many users are logged in.
	- **`load average: 0.00, 0.00, 0.00`**: How much demand there was on the system in the last 1, 5, and 10 minutes. 
	- **`Tasks`**: How many processes there are!
	- **`running`**, **`sleeping`**, etc.: How many processes are sleeping, running, etc. 
- Other general info includes how much RAM, swap space, CPU is being used, available, etc. 
- Below the general information is a list of processes. The general info and the process list are refreshed every 5 seconds.
- Commands you can run while in `top` screen include:

| Command | Action |
| --- | --- |
| **`h`** | For help. Press any key to go back to `top`. |
| **`M`** | Sort by memory. Press **`P`** to go back to default (CPU sorting). |
| **`1`** | Toggles all CPUs usage if the system has multiple CPUs. |
| **`R`** | Reverse sort the output. |
| **`u`** | Allows you to enter a specific username to list only his processes. |

- We usually use `top` to control rogue processes through renicing them (lowering their priorities) or killing them.
- To **renice** a process, with top, we first find the rogue process, we then press **`r`**. We enter its PID, and with the next prompt we enter the renice value (something between -19 and 20). We will see what this means later.
- To **kill** process, find its PID, then press **`k`**. When prompted, type **`15`** to cleanly kill the process, and **`9`** to forcefully kill it. We will see these magic numbers later. 

## Managing Foreground and Background Processes:
- To start a job in the background, you can type your command and follow it by an **`&`** as in:
```sh
python3 manage.py &
```
- When you press **`CTRL-Z`**, you stop a process. The process is still there, but it is sitting inactive in the background.
- To bring a job to the foreground, you'd use the **`fg`** command as in:
```sh
fg
```
- Without an argument, this simply brings the job that was most recently placed in the background.
- **`bg`** allows you to move a process to the background. You'd first have to stop it using `CTRL-Z`, then run `bg`.
- You can obviously place multiple processes in the background, and bring any of them to the foreground. The **`jobs`**, allows you to list background jobs (both running and stopped). It would output something like the following:
```
[1]   Running                 find / 2022 > /home/am/junk.txt &
[2]+  Stopped                 python3
[3]-  Running                 python3 f &
```
- You can use the information provided by the `jobs` command to see which which are running and which are stopped. You can also use their numbers to stop make them run, bring them foreground or stop them. To bring to foreground a job that 's not on top of the jobs stack you'd use its job number as follows. The following command will bring to foreground the job ` find / 2022 > /home/am/junk.txt &`:
```sh
fg %1
```
- The following table summarizes these commands:

| Command | Action |
| --- | --- |
| **`<some command> &`** | Start a process and have it run in the background. |
| **`CTRL+Z`** | Stop a foreground job (and place it in background). |
| **`fg`** | Bring to the foreground the most recently stopped process or job most recently placed in background. |
| **`fg %<job number>`** | Bring to the foreground the stopped or running job with the given number. |
| **`bg`** | Place in background the job that was most recently stopped |
| **`bg %<stopped job>`** | Make a stopped job run in the background. |
| **`jobs`** | List jobs (stopped processes/processes running in background). |

## Killing and Renicing Processes:
### Killing Processes with `kill` and `killall`:
- **`kill`** is used to kill a process by its PID.
- **`killall`** is used to kill multiple processes by command name. 
- These two commands don't just kill processes but can also send other signals. For more information about singal see **`man 7 signal`**. Signals are represented by both numbers and names as the following table shows:

| Signal  | Number | Action |
| --- | --- |
| **`SIGHUP`** | **`1`** | Hang-up detected on controlling terminal or death of controlling process. (*WHAT?!*) |
| **`SIGINT`** | **`2`** | Interrupt from keyboard |
| **`SIGQUIT`** | **`3`** | Quit from keyboard |
| **`SIGABRT`** | **`6`** | Abort |
| **`SIGKILL`** | **`9`** | Kill signal |
| **`SIGTERM`** | **`15`** | Termination signal (*Forced kill?!*) |
| **`SIGCONT`** | **`18`** | Continue a stopped process |
| **`SIGSTOP`** | **`19`** | Stops a process |

#### Using `kill` to Signal Processes by PID:
- To kill a process, let's a rogue one, we find its PID using `top` or `ps`, then give it to `kill` to kill it. This can be done with either of these three variations of the command:
```sh
kill 6666
kill -15 6666
kill -SIGKILL 6666
```

#### Using `killall` to Signal Processes by Name:
- `killall python3` will kill all processes whose name is `python3`.
- It is useful if you want to quickly kill multiple processes of the same command, but it can also kill processes you don't intend to kill. 

### Setting Process Priority with `nice` and `renice`:
- Every process has a so-called **`nice`** value between -19 and 20. This value is considered by the OS when its scheduling and giving CPU time to different processes. This value is set by default to 0. 
- Nice values behave as follows:
	- The lower the nice value, the higher priority it has. So process whose nice value is -5 has better access to the CPU than a process with the nice value 10.
	- A regular user cannot set a nice value below 0, meaning she cannot set it below the default value that all enjoy.
	- A regular user cannot lower the nice value. She can only make it larger, so she can only decrease access to CPU time.
	- A regular user can only change the nice value on her own processes. 
	- A root user can do whatever she wants. She can set the nice value higher and lower, and blow 0 and set any nice value in the system. 
- You can set a nice value when you run a command through the use of the **`nice`** command as in:
```sh
nice +5 updatedb &
```
- You can also change the nice value of an already running process with the **`renice`** command as in:
```sh
renice +7 1196
```
