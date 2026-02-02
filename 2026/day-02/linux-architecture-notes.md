## What is Linux and architecture?
Linux is an open-source operating system. It is the software that helps your computer talk to its hardware.
Linux is used in computers, servers, mobile phones, smart TVs, and even supercomputers.
The main purpose is also because 90% apps run on Linux OS.
Linux was first developed by Linus Torvalds in the year 1991.
  
## Architecture of Linux 
Linux architecture is how Linux is organised inside.
It has layers that work together step by step and each layer have specific job.
Main Layers of this Architecute is based on A S K H 

* Applications – Software like browsers, editors, tools.
* Shell – Takes user commands and sends them to the kernel.
*Kernel – The core of Linux; controls hardware and memory.
*Hardware – Physical parts like CPU, RAM, keyboard.
  
## Core Components of Linux
- **Kernel**
The kernel is the heart of Linux.
It controls hardware like CPU, memory, and devices.
All programs talk to hardware through the kernel.
- User Space
User space is where users and applications run.
It includes the shell, commands, and programs.

-init/systemd
Init is the **first process** started by the Linux kernel.
Modern Linux systems use systemd as the init system; it is the first program started after the Linux kernel.
systemd always runs as Process ID (PID) 1.
Manages services, boot process, and system state.

1. The computer is powered ON  
2. Linux kernel loads into memory  
3. Kernel starts **systemd**  
4. systemd runs as **Process ID 1 (PID 1)**  
5. systemd starts system services  
6. Network, login screen, and desktop start  
7. User can now use the system

## How processes are created and managed

A process is just a program that is running.
New processes are made by other processes using fork(), and they can start a new program with exec().
Every process has a unique PID and knows who its parent process is.
Each process gets its own memory and CPU time.
The system keeps track of all processes and manages them, making sure they run, wait, or stop when needed.

## Process States
- **Running (R)** – currently executing
- **Sleeping (S)** – waiting for input or resource
- **Uninterruptible Sleep (D)** – waiting for I/O
- **Stopped (T)** – paused manually
- **Zombie (Z)** – finished but not cleaned up by parent

## What systemd Does and Why It Matters
systemd is the first program that runs after the Linux kernel.
It starts and manages all other programs and services on the system.
It controls things like networking, logging, and running background tasks.
systemd keeps track of all processes, restarts them if they fail, and makes the system boot faster and more reliable.
For a DevOps engineer, knowing systemd is important because it helps manage services, troubleshoot failures, and automate system tasks.

## Common Daily Linux Commands
**mkdir**  
   - Create a new folder.  
   - Example: mkdir myfolder → creates a folder named "myfolder".

**touch**  
   - Create a new empty file or update file timestamp.  
   - Example: touch file.txt → creates "file.txt".

**vi**  
   - Open a file in terminal text editor.  
   - Example: vi file.txt → edit "file.txt".

**free -h**  
   - Check memory usage in human-readable format.  
   - Example: free -h → shows used and available RAM.

**ls**  
   - List files and folders in the current directory.  
   - Example: ls → shows all files/folders  
   - Example: ls -l → shows detailed list with permissions, size, date.
