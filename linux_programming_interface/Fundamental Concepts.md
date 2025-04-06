## The core operating system: The kernel

* central software managing a computer's resources + standard software tools
* central software managing a computer's resources

	The linux kernel executable typically in `/boot/vmlinuz`


### Tasks performed by the kernel
* Process scheduling
* Memory management
* Provision of a file system
* creation and termination of processes
* access to devices
* Networking
* Provision of a system call application programming interface (API)

### Kernel mode and user mode
virtual memory can be marked as being part of user space or kernel mode
if cpu in user mode cpu can access user mode memory
if cpu in kernel mode cpu can access user mode memory and kernel mode memory

certain operations can be performed only while the processor is operating in kernel mode (halt instruction, accessing memory-management hardware, initiating device I/O operations)

## The Shell

A `shell` is special-purpose program designed to read commands typed by a user and execute appropriate programs in response to those commands

`login shell` = denote the process that is created to run a shell when the user first logs in

* Bourne shell (sh)
* C shell (csh)
* Korn shell (ksh)
* Bourne again shell (bash)

## Users and Groups

Each user on the system is uniquely identified, and users may belong to groups

**Users**

Every user of the system has unique login name (username) and a corresponding numeric user ID (UID). For each user, these are defined by a line in the system `password` file `/etc/password` which includes
* `Group ID`
* `Home directory`
* `Login Shell`

The password record may also include the user's password, in encrypted form. However, for security reasons, the password is often stored in the sperate `shadow password file`, which is readable only by privilieged users

**Groups**

For administrative purposes for controlling access to files and other system resources it is useful to organize users into `groups`

Each group is identified by a single line in the system `group file`, `/etc/group` which includes
* `Group name`
* `Group ID (GID)`
* `User list`

**SuperUser**

has special privileages within the system.
ID 0, login name `root`

the superuser bypasses all permission checks in the system.

## Single Directory Hierarchy, Directories, Links, and Files

At the base of this hierarchy is the `root(/)`. All files and directories are children or further removed descendants of the root

``` mermaid
graph TD
/ --> bin
	bin --> bash
/ --> boot
	boot --> vmlinuz
/ --> etc
	etc --> group
	etc --> passwd
/ --> home
/ --> usr
	usr --> include
		include --> sys
		include --> stdio.h
			sys --> types.h
```

**File types**

each file is marked with a `type`, indicating what kind of file it is. (`regular`, `devices`, `pipes`, `sockets`, `directories`, `symbolic links` ...)

**Directories and links**
`directory` is a special file whose contents take the form of a table of filenames coupled with references to the corresponding files.

link = `filename + reference`, file may have multiple links, and multiple names

every directory contains `.` `..` which is a links to the directory itself and parent directory

**Symbolic links**

provides an alternative name for a file

normal link is a filename-plus-pointer entry in a directory list, a symbolic link is a specially marked file containing the name of another file.

**Filenames**

On most linux file systems, filenames can be up to 255 characters long

If a filename containing characters with special meanings appears in such contexts, the these characters must be escaped; using `\` (backslash) to indicate that they should not be interperted with those special meanings.

**Pathnames**

A pathname is a string consisting of an optional initial slash (/) followed by a seris of filenames separated by slashes.

* `absolute pathname`: begins with a slash(/) and specifies the location of a file with repect to the root directory. (/usr/include, /home/username)
* `relative pathname`: specifies the location of a file relative to a process's current working directory(../my_file, ./my_other_file)

**Current working directory**

Each process has a current working directory (process's working directory) this is the process's "current location"

shell's current working directory can be changed with the cd command

**File ownership and Permissions**

Each file has an associated user ID and group ID that define the owner of the file and the group to which it belongs

## FILE I/O Model

same system calls (`open()`, `read()`, `write()`, `close()` ...) are used to perform I/O on all types of files, including devices

**File descriptors**

The I/O system calls refer to open files using a `file descriptor` a non-negative integer. A file descriptor is typically obtained by a call to open(), which takes a pathname argument specifying a file upond which I/O is to be performed

* descriptor 0 : standard input
* descriptor 1 : standard output
* descriptor 2 : standard error

these three descriptors are normally connected to the terminal. In the `stdio` library, these descriptors correspond to the file streams `stdin, stdout, stderr`

**The `stdio` library**

To perform file I/O, C programs typically employ I/O functions contained in the standard C library, referred to as the `stdio` library

includes: `fopen(), fclose(), scanf(), printf(), fgets(), fputs() ...`

the `stdio` functions are layered on top of the I/O system calls (`open(), close(), read(), write(), ...`)

## Programs

`programs` normally exist in two forms. `source code` and `binary (executable)`

**Filters**

A filter is the name often applied to a program that reads its input from `stdin`, performs some transformation of that input, and writes the transformed data to `stdout` (`grep, tr, sort, wc, sed, awk`)

**Command-line arguments**

In C, programs can access the command-line arguments, the words that are supplied on the command line when the program is run.

`int main(int argc, char *argv[])`

The `argc` variable contains the total number of command-line arguments, and the individual arguments are available as strings pointed to by members of the array `argv` The first of these strings, `argv[0]`, identifies the name of the program itself

## Processes

`process` is an instance of an executing program.

When a program is executed, the kernel loads the code of the program into virtual memory, allocates space for program variables, and sets up kernel bookkeeping data structures to record various information(process ID, termination status, user IDs, and group IDs) about the process

**Process memory layout**

A process is logically divided into the following parts, known as `segments:`

* Text: the instructions of the program
* Data: the static variables used by the program
* Heap: an area from which programs can dynamically allocate extra memory
* Stack: a piece of memory the grows and shrinks as functions are called and return and that is used to allocate storage for local variables and function call linkage information

**Process creation and program execution**

A process can create a new process using the `fork()` system call new process is referred to as the `child process` the kernel creates the child process by making a duplicate of the parent process. The child inherits copies of the parent's data, stack, heap segments

The child process goes on either to execute a different set of functions in the same code as parent, or, frequently, to use the `execue()` system call to load and execute an entirely new program.

`execue()` call destroys the existing text, data, stack, and heap segments, replacing them with new segments based on the code of the new program

**Process ID and parent process ID**

Each process has a unique integer process identifier (PID). Each process also has a parent process identifier(PPID)

**Process termination and termination status**

A process can terminate in one of two ways

1. by requesting its own termination use the `exit()` system call
2. being killed by the delivery of a signal

In either case, the process yields a `termination status`, a small nonnegative integer value that is avaliable for inspection by the parent process using the `wait()` system call

By convention, a termination status of 0 indicates that the process succeeded, and a nonzero status indicates that some error occurred.

Most shells make the termination status of the last executed program available via a shell variable named `$?`

**Process user and group identifiers (credentials)**

Each process has a number of associated user IDs (UIDs) and group IDs (GIDs)

* Real user ID and real group ID
* Effective user ID and effective group ID: used in determining the permissions that the process has when accessing protected resources such as files and interprocess communication objects
* Supplementary group IDs: identify additional groups to which a process belongs. A new process inherits its supplementary group IDs from its parent. A login shell gets it supplementray group IDs from the system group file.

**Privileged processes**

a `privileged process` is on whose `effective` user ID is 0 (superuser).

**Capabilities**

Each privileged operation is associated with a particular capability, and a process can perform an operation only if it has the corresponding capability. A traditional superuser process corresponds to a process with all capabilities enabled.

**The `init` process**

When booting the system, the kernel creates a special process called `init`, the "parent of all processes," which is derived from the program file `/sbin/init` All processes on the system are created (using fork()) either by `init` or by one of its descendants.

The `init` process always has the process ID 1 and runs with superuser privileges.

The `init` process can't be killed, and it terminates only when the system is shut down.

The main task of `init` is to create and monitor a range of processes required by a running system

**Daemon processes**

A `daemon` is a special-purpose process that is created and handled by the system in the same way as other processes, but which is distinguished by the following characteristics

* It is long-lived. A daemon process is often started at system boot and remains in existence until the system is shut down.
* It runs in the background, and has no controlling terminal from which it can read input or to which it can write output.

`syslogd`, `httpd`, ....

**Environment list**

Each process has an `environment list`, which is set of `environment variables` that are maintained within the user-space memory of the process.

When a new process is created via `fork()`, it inherits a copy of its parent's environment. Thus, the environment provides a mechanism for a parent process to communicate information to child process. When a process replaces the program the it is running using `exec()`, the new program either inherits the environment used by the old program or receives a new environment specified as part of the `exec()` call.

Environment variables are created with the `export` command in most shells

`export MYVAR='Hello world'`

C programs can access the environment using an external variable (`char **environ),

**Resource limits**

Each process consumes resources, such as open files, memory, and CPU time. Using `setrlimit()` system call, a process can establish upper limits on its consumption of various resources.

* soft limit: limits the amount of the resource that the process may consume;
* hard limit: which is a ceiling on the value to which the soft limit may be adjusted. An unprevileged process may change its soft limit for a particular resource to any value in the range from zero up to the corresponding hard limit.

When a new process is created with `fork()`, it inherits copies of its parent's resource limit settings.

The resource limits of the shell can be adjusted using the `ulimit` command (`limit` in C shell).

## Memory Mappings

The `mmap()` system call creates a new `memory mapping` in the calling process's virtual address space

* A `file mapping` maps a region of a file into the calling process's virtual memory.
* `anonymous mapping` doesn't have a corresponding file. Instead, the pages of the mapping are initialized to 0.






