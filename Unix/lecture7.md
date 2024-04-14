# Unix Lecture 7

## Process Control

#### 2023.12.18.(월)

### Process Identifiers

Every process has a unique process ID, a non-negative integer. As processes terminate, their IDs can be reused. Most UNIX systems implement algorithms to delay reuse so that newly created processes are assigned IDs different from those used by processes that terminated recently. This prevents a new process from being mistaken for the previous process to have used the same ID.

There are some special processes, but the details differ from implementation to implementation:

- Process ID 0: scheduler process (often known as the `swapper`), which is part of the kernel and is known as a system process

- Process ID 1: `init` process, invoked by the kernel at the end of the bootstrap procedure.
  - It is responsible for bringing up a UNIX system after the kernel has been bootstrapped. `init` usually reads the system-dependent initialization files (`/etc/rc*` files or `/etc/inittab` and the files in `/etc/init.d`) and brings the system to a certain state.
  - It never dies.
  - It is a normal user process, not a system process within the kernel.
  - It runs with superuser privileges.

Each UNIX System implementation has its own set of kernel processes that provide operating system services. For example, on some virtual memory implementations of the UNIX System, process ID 2 is the pagedaemon. This process is responsible for supporting the paging of the virtual memory system.

```C
#include <unistd.h>

pid_t getpid(void);
// Returns: process ID of calling process

pid_t getppid(void);
// Returns: parent process ID of calling process

uid_t getuid(void);
// Returns: real user ID of calling process

uid_t geteuid(void);
// Returns: effective user ID of calling process

gid_t getgid(void);
// Returns: real group ID of calling process

gid_t getegid(void);
// Returns: effective group ID of calling process
```

None of these functions has an error return.

### fork function

An existing process can create a new one by calling the `fork` function.

```C
#include <unistd.h>

pid_t fork(void);
// Returns: 0 in child, process ID of child in parent, -1 on error
```

- The new process created by `fork` is called the child process. This function is called once but returns twice.
  The only difference in the returns is that the return value in the child is 0, whereas the return value in the parent is the process ID of the new child.

  - `fork` returns child's process ID in parent: a process can have more than one child, and there is no function that allows a process to obtain the process IDs of its children
  - `fork` returns 0 in child: a process can have only a single parent, and the child can always call `getppid` to obtain the process ID of its parent

- Both the child and the parent continue executing with the instruction that follows the call to `fork`. The child is a copy of the parent. For example, the child gets a copy of the parent's data space, heap, and stack. Note that this is a copy for the child the parent and the child do not share these portions of memory. The parent and the child do share the text segment.

- Copy-on-write(COW) is used on modern implementations: a complete copy of the parent's data, stack and heap is not performed. The shared regions are changed to read-only by the kernel. The kernel makes a copy of that piece of memory only if either process tries to modify these regions.

Variations of the `fork` function are provided by some platforms. All four platforms discussed in this book support the `vfork(2)` variant discussed in the next section.
Linux 3.2.0 also provides new process creation through the `clone` system call. This is a generalized form of `fork` that allows the caller to control what is shared between parent and child.

```C
#include "apue.h"

int globvar = 6; // external variable in initialized data
char buf[] = "a write to stdout\n";

int main(void) {
    int var; // automatic variable on the stack
    pid_t pid;

    var = 88;
    if (write(STDOUT_FILENO, buf, sizeof(buf) - 1) != sizeof(buf) - 1)
      err_sys("write error");

    printf("before fork\n"); // we don't flush stdout

    if ((pid = fork()) < 0) {
        err_sys("fork error");
    } else if (pid == 0) {
        globvar++;
        var++;
    } else {
        sleep(2);
    }

    printf("pid = %ld, glob = %d, var = %d\n", (long)getpid(), globvar, var);

    exit(0);
}
```

### File Sharing

One characteristic of `fork` is that all file descriptors that are open in the parent are duplicated in the child, because it's as if the `dup` function had been called for each descriptor. The parent and the child shareafile table entry for every open descriptor.

For a process that has three different filees opened for standard input, standard output, and standard error, on return from `fork`, we have the arrangement shown below:

It is important that the parent and the child share the same file offset. Otherwise, this type of interaction would be more difficult to accomplish and would require explicit actions by the parent.

There are two normal cases for handling the descriptors after a `fork`:

1. The parent waits for the child to complete.
2. Both the parent and the child go their own ways. After the fork, both the parent and child close the descriptors that they don't need, so neither interferes with the other's open descriptors. This scenario is often found with network servers.

Besides the open files, other properties of the parent are inherited by the child:

- Real user ID, real group ID, effective user ID, and effiective group ID
- Supplementary group IDs
- Process group ID
- Session ID
- Controlling terminal
- The set-user-ID and set-group-ID flags
- Current working directory
- Root directory
- File mode creation mask
- Signal mask and dispositions
- The close-on-exec flag for any open file descriptors
- Environment
- Attached shared memory sements
- Memory mappings
- Resource limits

The differences between the parent and child are:

- The return values from fork are different.
- The process IDs are different.
- The two processes have different parent process IDs: the parent process ID of the child is the parent; the parent process ID of the parent doesn't change.
- The child's `tms_utime`, `tms_stime`, `tms_cutime`, and `tms_cstime` values are set to 0.
- File locks set by the parent are not inherited by the child.
- Pending alarms are cleared for the child.
- The set of pending signals for the child is set to the empty set.

### The two main reasons for `fork` to fail

1. If too many processes are already in the system, which usually means that something else if wrong.

2. If the total number of processes for this real user ID exceeds the system's limit.

### The two uses for `fork`

1. When a process wants to duplicate itself so that the parent and the child can each execute different sections of code at the same time.

- This is common for network servers - the praent waits for a service request from a client. When the request arrives, the parent calls `fork` and lets the child handle the request. The parent goes back to waiting for the next service request to arrive.

2. When a process wants to execute a different program.

- This is common for shells. In this case, the child does an `exec` right after it returns from the `fork`.

Some operating systems combine the operations from step2, a `fork` followed by an `exec`, into a single operation called a `spawn`. The UNIX System separates the two, as there are numerous cases where it is useful to `fork` without doing an `exec`. Also, separating the two operations allows the child to change the per-process attributes between the `fork` and the `exec`, such as I/O redirection, user ID, signal disposition, and so on

### `vfork` Function

The function `vfork` has the same calling sequence and same return values as `fork`, but the semantics of the two functions differ.

The `vfork` function was intended to create a new process for the purpose of executing a new program (step 2 at the end of the previous section). The `vfork` function creates the new process, just like `fork`, without copying the address space of the parent into the child, as the child won't reference that address space; the child simply calls `exec` (or `exit`) right after the `vfork`. Instead, the child runs in the address space of the parent until it calls either `exec` or `exit`.

This optimization is more efficient on some implementations of the UNIX System, but leads to undefined results if the child:

- modifies any data (except the variable used to hold the return value from `vfork`)
- makes function calls
- returns without calling `exec` or `exit`

Another difference between the two functions is that `vfork` guarantees that the child runs first, until the child calls `exec` or `exit`. When the child calls either of these functions, the parent resumes.
(This can lead to deadlock if the child depends on further actions of the parent before calling either of these two functions.)

The program is a modified versioin of the program from fork1.c. We've replaced the call to `fork` with `vfork` and removed the write to standard output. Also, we don't need to have the parent call `sleep`, as we're guaranteed that it is put to sleep by the kernel until the child calls either `exec` or `exit`.

```C
#include "apue.h"

int globvar = 6;

int main(void)
{
    int var;
    pid_t pid;

    var = 88;
    printf("before vfork\n");
    if ((pid = vfork()) < 0) {
        err_sys("vfork error");
    } else if (pid == 0) {
        globvar++;
        var++;
        _exit(0);
    }

    printf("pid = %ld, glob = %d, var = %d\n", (long)getpid(), globvar, var);
    exit(0);
}
```

Most modern implementations of `exit` do not close the streams. Because the process is about to exit, the kernel will close all the file descriptors open in the process. Closing them in the library simply adds overhead without any benefit.

### `exit` Functions

A process can terminate normally in five ways.

1. Executing a return from the `main` function. This is equivalent to calling `exit`.
2. Calling the `exit` function, which includes the calling of all exit handlers that have been registered by calling `atexit` and closing all standard I/O streams.

- ISO C does not deal with file descriptors, multiple processes (parents and children), and job control. The definition of this function is incomplete for a UNIX system.

3. Calling the `_exit` or `_Exit` function.

- `_Exit`: defined by ISO C to provide a way for a process to terminate without running exit handlers or signal handlers
- `_exit`: called by `exit` and handles the UNIX system-specific details; `_exit` is specified by POSIX.1.
- Whether standard I/O streams are flushed depends on the implementation.
- On UNIX systems, `_Exit` and `_exit` are synonymous and do not flush standard I/O streams.

4. Executing a `return` from the start routine for the last thread in the process.

- The return value of the thread is not used as the return value of the process. When the last thread returns from its start routine, the process exits with a termination status of 0.

5. Calling the `pthread_exit` function from the last thread in the process.

The three forms of abnormal termination:

1. Calling `abort`. This is a special case of the next item, as it generates the `SIGABRT` signal.
2. When the process receives certain signals. The signal can be generated by:

- the process itself, e.g., calling the `abort` function
- some other processes
- the kernel, e.g. the process references a memory location not within its address space or tries to divide by 0

3. The last thread responds to a cancellation request. By default, cancellation occurs in a deffered manner: one thread requests that another be canceled, and sometime later the target thread terminates.

Regardless of how a process terminates, the same code in the kernel is eventually executed. This kernel code closes all the open descriptors for the process, releases the memory that it was using, and so on.

The terminating process is to be able to notify its parent how it terminated by by passing an exit status as the argument to one of the three exit functions. In the case of an abnormal termination, the kernel (not the process) generates a termination status to indicate the reason for the abnormal termination. In any case, the parent of the process can obtain the termination status from either the `wait` or the `waitpid` function.

### Exit status vs. termination status

- Exit status: is the argument to one of the three exit functions or the return value from main.

- Terminatioin status: the exit status is converted into a termination status by the kernel when `_exit` is finally called. If the child terminated normally, the parent can obtain the exit status of the child.

### Orphan process

`Orphan process` (or orphaned child process) is any process whose parent terminates.

The child has a parent process after the call to `fork`. What happens if the parent terminates before the child? The answer is the `init` process becomes the parent process of any process whose parent terminates.
This is called "the process has been inherited by `init`". Whenever a process terminates, the kernel goes through all active processes to see whether the terminating process is the parent of any process that still exists. If so, the parent process ID of the surviving process is changed to be 1 (the process ID of `init`). This way, it's guaranteed that every process has a parent.

### Zombie process

`Zombie process` is a process that has terminated, but whose parent has not yet waited for it. The `ps` command prints the state of a zombie process as Z.

What happens when a child terminates before its parent?

If the child completely disappeared, the parent wouldn't be able to fetch its terminatioin status when and if the parent was finally ready to check if the child had terminated. The kernel keeps a small amount of information for every terminating process, so that the information is available when the parent of the terminating process calls `wait` or `waitpid`.
This information consists of the process ID, the termination status of the process, and the amount of CPU time taken by the process. The kernel can discard all the memory used by the process and close its open files.

### `init`'s children

What happens when a process that has been inherited by `init` terminates? It does not become a zombie. `init` is written so that whenever one of its children terminates, `init` calls one of the `wait` functions to fetch the termination status. By doing this, init prevents the system from being clogged by zombies.

One of init's children refers to either of the following:

- A process that `init` generates directly (e.g. getty)
- A process whose parent has terminated and has been subsequently inherited by `init`.

### `wait` and `waitpid` Functions

When a process terminates, either normally or abnormally, the kernel notifies the parent by sending the `SIGCHLD` signal to the parent. Because the termination of a child is an asynchronous event (it can happen at any time while the parent is running). This signal is the asynchronous notification from the kernel to the parent. The parent can choose to ignore this signal, or it can provide a function that is called when the signal occurs: a single handler. The default action for this signal is to be ignored.

A process that calls `wait` or `waitpid` can:

- Block, if all of its children are still running
- Return immediately with the termination status of a child, if a child has terminated and is waiting for its termination status to be fetched
- Return immediately with an error, if it doesn't have any child processes

If the process is calling `wait` because it received the `SIGCHLD` signal, we expect wait to return immediately. But if we call it at any random point in time, it can block.

```C
#include <sys/wait.h>

pid_t wait(int *statloc);
pid_t waitpid(pid_t pid, int *statloc, int options);

// Return: process ID if OK, 0 (see later), or -1 on error
```

The differences between these two functions are:

- The `wait` function can block the caller until a child process terminates, whereas `waitpid` has an option that prevents it from blocking.

- The `waitpid` function doesn't wait for the child that terminates first; it has a number of options that control which process it waits for.

### `wait` for a specific process: `waitpid`

If we have more than one child, `wait` returns on termination of any of the children. If we want to wait for a specific process to terminate (assuming we know which process ID we want to wait for), in older versions of the UNIX System, we would have to call `wait` and compare the returned process ID with the one we're interested in. The POSIX.1 `waitpid` function can be used to wait for a specific process.

The interpretation of the pid argument for waitpid depends on its value:

- pid == -1 : Waits for any child process. In this respect, `waitpid` is equivalent to `wait`.
- pid > 0 : Waits for the child whose process ID equals pid.
- pid == 0 : Waits for any child whose process group ID equals that of the calling process.
- pid < -1 : Waits for any child whose process group ID equals the absolute value of pid.

### `exec` Functions

One use of the `fork` function is to create a new process (the child) that then causes another program to be executed by calling one of the `exec` functions.

- When a process calls one of the `exec` functions, that process is completely replaced by the new program which starts executing at its `main` function.
- The process ID does not change across an `exec`, because a new process is not created.
- `exec` merely replaces the current process (its text, data, heap, and stack segments) with a brand-new program from disk.

UNIX System process control primitives:

- `fork` creates new processes
- `exec` functions initiates new programs
- `exit` handles termination
- `wait` functions handle waiting for termination

There are seven different `exec` functions:

```C
#include <unistd.h>

int execl(const char *pathname, const char *arg0, ... /* (char *)0 */);
int execv(const char *pathname, char *const argv[]);
int execle(const char *pathname, const char *arg0, ...
/* (char *)0, char *const envp[] */);
int execve(const char *pathname, char *const argv[], char *const envp[]);
int execlp(const char *filename, const char *arg0, ... /* (char *)0 */);
int execvp(const char *filename, char *const argv[]);
int fexecve(int fd, char *const argv[], char *const envp[]);

// All seven return: -1 on error, no return on success
```

### Changing User IDs and Group IDs

In the UNIX System, privileges and access control are on user and group IDs. When programs need additional privileges or access to unallowed resources, they need to change their user or group ID to an ID that has the appropriate privilege or access. It is similar when the programs need to lower their privileges or prevent access to certaiin resources.

When designing applications, we try to use the least-privilege model, which means our programs should use the least privilege necessary to accomplish any given task. This reduces the risk that security might be compromised by a malicious user trying to trick our programs into using their privileges in unintended ways.

We can set the real user ID and effective user ID with the `setuid` function and set the real group ID and the effective group ID with the `setgid` function.

```C
#include <unistd.h>

int setuid(uid_t uid);
int setgid(gid_t gid);

// Return: 0 if OK, -1 of error
```

The rules for who can change the IDs, considering only the user ID now (Everything we describe for the user ID also applies to the group ID)

1. If the process has superuser privileges, the `setuid` function sets the real user ID, effective user ID, and saved set-user-ID to uid.

2. If the process does not have superuser privileges, but uid equals either the real user ID or the saved set-user-ID, setuid sets only the effective user ID to uid. THe real user ID and the saved set-user-ID are not changed.

3. If neither of these two conditions is true, `errno` is set to `EPERM` and -1 is returned.

### `setreuid` and `setregid` Functions

Historically, BSD supported the swapping of the real user ID and the effective user ID with the setreuid function.

```C
#include <unistd.h>

int setreuid(uid_t ruid, uid_t euid);
int setregid(gid_t rgid, gid_t egid);

// Return: 0 if OK, -1 on error
```

- A value of -1 for any of the arguments indicates that the corresponding ID should remain unchanged.

- An unprivileged user can always swap between the real user ID and the effective user ID. This allows a set-user-ID program to swap to the user's normal permissions and swap back again later for set-user-ID operations.

- When the saved set-user-ID feature was introduced with POSIX.1, the rule was enhanced to also allow an unprivileged user to set its effective user ID to its saved set-user-ID.

### `seteuid` and `setegid` Functions

POSIX.1 includes `seteuid` and `setegid` that only change the effective user ID or effective group ID.

```C
#include <unistd.h>

int seteuid(uid_t uid);
int setegid(gid_t gid);

// Both Return: 0 if OK, -1 on error
```

- An unprivileged user can set its effective user ID to either its real user ID or its saved set-user-ID.

- For a privileged user, only the effective user ID is set to uid. This differs from `setuid` function, which changes all three user IDs.

### `seteuid` and `setegid` Functions

POSIX.1 includes `seteuid` and `setegid` that only change the effective user ID or effective group ID.

```C
#include <unistd.h>

int seteuid(uid_t uid);
int setegid(gid_t gid);

// Both return: 0 if OK, -1 on error
```

- An unprivileged user can set its effective user ID to either its real user ID or its saved set-user-ID.

- For a privileged user, only the effective user ID is set to uid. This differs from `setuid` function, which changes all three user IDs.

### Group IDs

Everything covered so far for user IDs in this section also applies in a similar fashion to group IDs. The supplementary group IDs are not affected by `setgid`, `setregid`, or `setegid`.

### `system` Function

It is convenient to execute a command string from within a program.

```C
#include <stdlib.h>

int system(const char *cmdstring);
```

Arguments:

If cmdstring is a null pointer, system returns nonzero only if a command processor is available, which determines whether the `system` function is supported on a given platform. Under UNIX systems, it is always available.

Return values:

Since `system` is implemented by calling `fork`, `exec`, and `waitpid`, there are three types of return values:

1. If either the `fork` fails or `waitpid` returns an error other than `EINTR`, `system` returns -1 with `errno` set to indicate the error.

2. If the `exec` fails, implying that the shell can't be executed, the return value is as if the shell had executed `exit`.

3. If all three functions succeed, the return value is the termination status of the shell, in the format specified for `waitpid`.

The code below is an implementation of the `system` function, which doesn't handle signals.

```C
#include <sys/wait.h>
#include <errno.h>
#include <unistd.h>

int system(const char *cmdstring)
{
    pid_t pid;
    int status;

    if (cmdstring == NULL)
        return (1);

    if ((pid = fork()) < 0) {
        status = -1;
    } else if (pid == 0) {
        execl("/bin/sh", "sh", "-c", cmdstring, (char *)0);
        _exit(127);
    } else {
        while (waitpid(pid, &status, 0) < 0) {
            if (errno != EINTR) {
                status = -1;
                break;
            }
        }
    }

    return (status);
}
```

### User Identification

Any process can find out its real and effective user ID and group ID. `getpwuid(getuid())` can be used to find out the login name of the user who's running the program. However, a single user can have multiple login names, that is, a person might have multiple entries in the password file with the same user ID to have a different login shell for each entry. The system normally keeps track of the name we log in under the `utmp` file, and the `getlogin` function provides a way to fetch that login name.

```C
#include <unistd.h>

char *getlogin(void);

// Returns: pointer to string giving login name if OK, NULL on error
```

This function can fail if the process is not attached to a terminal that a user logged in to. We normally call these processes `daemons`.

Given the login name, we can then use it to look up the user in the password file (e.g. to determine the login shell) using `getpwnam`.

The environment variable `LOGNAME` is usually initialized with the user's login name by `login(1)` and inherited by the login shell. However, a user can modify an environment variable, so we shouldn't use `LOGNAME` to validate the user in any way. Instead, we should use `getlogin`.

### Process Scheduling

Historically, the UNIX System provided processes with only coarse control over their scheduling priority. The scheduling policy and priority were determined by the kernel.

- A process could choose to run with lower priority by adjusting its nice value

  - A process could be "nice" and reduce its share of the CPU by adjusting its nice value

- Only a privileged process was allowed to increase its scheduling priority.

In the Single UNIX Specification, nice values range from 0 to `(2*NZERO) - 1`, although some implementations support a range from 0 to `2*NZERO`. Lower nice values have higher scheduling priority. Lower nice values have higher scheduling priority. "The more nice you are, the lower your scheduling priority is." `NZERO` is the default nice value of the system.

A process can retrieve and change its nice value with the `nice` function. With this function, a process can affect only its own nice value; it can't affect the nice value of any other process.
