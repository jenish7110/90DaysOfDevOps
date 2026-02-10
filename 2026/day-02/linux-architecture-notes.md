Core Components of Linux

(Kernel, User Space, Init/Systemd)

Kernel

The kernel is the heart of Linux. Below is the complete Linux architecture:

Application
    |
Shell
    |
Kernel (Converts shell commands into executable code that hardware can execute)
    |
Hardware


Hardware never accepts input directly. Input is given via the shell, passed to the kernel, and then interpreted and executed on the hardware.

User Space

Everything that is not part of the kernel is called User Space.

Examples:

Shell commands (ls, pwd)

Environment (Python, Java)

Database

Application server

Init System (Systemd)

After the kernel boots, it needs a system to properly start up the system — this is systemd.

Systemd:

First process started by Linux

PID is always 1

Responsibilities:

Start system services

Manage service lifecycle

Handle dependencies

Restart failed services

Manage system state

Logging

Examples:
systemctl start nginx
systemctl status docker
journalctl kubelet


systemctl manages everything that runs on the system.

One-liner:

Linux consists of the kernel, which manages hardware and system resources.

How Processes Are Created and Managed
What is a Process?

A program is a file on disk

When a program runs, it becomes a process

Process = Program (in memory) + executable state


Each process has:

PID (Process ID)

PPID (Parent Process ID)

Memory space

File descriptors

User permissions

State (Running, Sleeping, etc.)

Every process has a parent process except systemd (PID 1).

Process Creation

A parent process creates a process using fork()

The child process usually calls exec()

Example:

bash -> fork() -> child -> exec(ls)


fork() creates a new process

exec() loads a program into that process

Process Lifecycle States

R – Running

S – Sleeping (waiting for I/O)

D – Uninterruptible sleep

T – Stopped

Z – Zombie

Commands:
ps aux
top
htop

Process Termination

Every process has a parent except systemd.

If a parent dies:

The child is adopted by systemd

ps -ef --forest

Process Termination Conditions:

Finishes normally and exits

Receives a signal

Zombie Process

Process has exited but the parent didn’t collect the exit status

PID exists

No memory or CPU usage

State = Z

Fix:

Restart or kill the parent process

systemd usually cleans these up

Process Management by the Kernel

The kernel handles:

CPU scheduling (which process runs)

Context switching

Memory isolation

Priority and nice values

Priority Examples:
nice -n 10 myapp
renice -5 PID


Higher priority is given to the process.

Signal Control

Signals control process behavior:

SIGTERM – Graceful stop

SIGKILL – Force kill

systemd and Process Management

systemd:

Starts services

Restarts failed processes

Tracks main PID

Enforces limits

Linux processes are created using the fork and exec model, managed by the kernel through scheduling, signals, and memory isolation, and supervised by systemd, with PID 1 as the ancestor of all processes.

What systemd Does and Why It Matters

systemd is an init system and service manager in Linux.

After the kernel boots:

systemd is the first user-space process

Always has PID 1

Brings the system from booted → usable

1. System Startup

systemd:

Starts services in the correct order

Handles dependencies automatically

Starts services in parallel (fast boot)

2. Service Management

Start, stop, restart services

Tracks main PID

Automatically restarts crashed services

3. Dependency and Ordering Management

Defines what a service requires

Defines what starts after another service

Ensures no service starts before its dependencies

4. Failure Handling and Self-Healing

Restarts services on failure

Applies restart limits

Marks system as degraded if a critical service fails

5. Logging (journald)

Structured logs

Indexed

Pre-service logs available

Example:

journalctl -n nginx

6. Resource Control

Places services into cgroups

Controls CPU, memory, and I/O limits

7. System State Management

systemd manages the overall system state.

Final Summary:

systemd is the Linux init system and service manager that controls booting and service lifecycles, handles dependencies, logging, and resource limits—making modern Linux systems fast, reliable, and observable.