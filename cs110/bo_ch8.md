# [Bryant & O'Hallaron, Ch. 8](http://www.codeman.net/wp-content/uploads/2011/12/Computer_Systems-A_Programmers_Perspective-2e.pdf#page=736): Exceptional Control Flow

- When a processor is first powered on, the program counter assumes a sequence of values like $a_0, a_1, \dots, a_{n-1}$. Each of these values is the address of some corresponding instruction $I_k$.
- Each transition from $a_k$ to $a_{k+1}$ is called a control transfer. A sequence of control transfers is called the **control flow** of the processor.
- Packets arriving at a network adapter, signals from processes, or parent processes waiting on a child to terminate all causes changes to the control flow. These changes are known as **exceptional control flow** (ECF).
- Understanding ECF is necessary to understand how applications interact with the OS and how concurrency and software exceptions work.

## Ch. 8.1: Exceptions
- An **exception** is an abrupt change in the control flow in response to some change in the processor's state (i.e. an event occurs).
- Control goes from application program to an exception handler that does some processing, then either returns control to the program or aborts it.
- An exception table holds handler code for all exceptions. This jump table is initialized at system boot time.
- Closely related to both software and hardware.
- There are four classes of exceptions (only interrupts are asynchronous; the rest are synchronous): 
1. Interrupts
- Occur asynchronously (not caused by execution of a particular instruction) as a result of signals from I/O device
2. Traps
- Intentional exceptions that occur as a result of executing an instruction. 
- Occur when system calls are made to access kernel services. System call parameters are passed through registers.
- Returns control to next instruction.
- Popular system calls:
![](https://image.ibb.co/dWF3Vd/Screen_Shot_2018_05_10_at_3_21_41_PM.png)
3. Faults
- Result from error conditions that a handler might be able to correct.
- If correctable, returns control to current instruction to reexecute it. Otherwise, aborts.
4. Aborts
- Unrecoverable fatal errors like hardware issues.

## Ch. 8.2: Processes
- **Process** = an instance of a program in execution. Each program runs in the context of some process.
- Each time a user runs a program by entering the name of some executable, the shell creates a new process.
- Processes work because of logical control flow and a private address space.

### Logical Control Flow
- Logical flow represents the sequence of program counter values that correspond to instructions exclusively in our program's executable file or related ones.
- Processes take turns using the processor (called **multitasking**).
- **Concurrent flow:** = logical flow whose executions overlaps in time with another flow. Those overlapping flows run concurrently.
- **Parallel flow:** = when two flows are running concurrently on different processor cores or computers.

### Private Address Space
- A process cannot accss any other process's address space.
![](https://image.ibb.co/b84Nqd/Screen_Shot_2018_05_10_at_3_34_25_PM.png)
- Bottom of the stack has the highest address and the top has the lowest.

### Context Switches
- The OS kernel implements multitasking using context switching.
- The kernel maintains a context for each process. The context is the state that the kernel needs to restart a preempted process (i.e. registers, program counter, stacks, file entry table, etc.). 
- At certain points during process execution, the kernel can decide to pause the current process and restart a previously paused one. This is handled by the **scheduler**.

## Ch. 8.3: System Call Error Handling
- When system calls encounter an error, they return -1 and set the global variable `errno` to indicate what went wrong.
- You can use `strerror(errno)` to retrieve a text string that describes the error associated with a particular `errno` value.
- An error-reporting function is what you call to do the meat of your error-checking.
- An error-handling wrapper handles both the call and error-checking (i.e. `Fork()` instead of `fork()`)

## Ch. 8.4: Process Control

### Obtaining Process IDs
- Each process has a unique positive process ID (pid).
#### `pid_t getpid()` and `pid_t getppid()` 
- Returns the PID of the caller and the parent, respectively

### Creating and Terminating Processes
- Processes are in one of three states:
1. Running (being executed by CPU, or waiting to be scheduled by kernel).
2. Stopped (when SIGSTOP, SIGTSTP, SIGTTIN, or SIGTTOU signal is received). Process remains suspended until a SIGCONT signal is received.
3. Terminated (like when `exit(int status)` is called; `exit()` doesn't return but instead sets the exit status in `status`).

#### `pid_t fork(void)`
- How a parent process creates a new running child process.
- Returns 0 to child, child pid to parent, -1 on error.
- Call once, return twice.
- Concurrent execution (don't assume anything about the interleaving of processes because it's nondeterministic!)
- Duplicate but separate address spaces.
- Shared file descriptors.

#### Reaping Child Processes
- When a process terminates, the kernel doesn't remove it from the system immediately. Instead, the **zombie process** is kept around until it's reaped by its parent. Zombie processes consume system memory resources.
- When the parent reaps the terminated child, the kernel passes the child's exit status to the parent and then discards the terminated process.
- We have to **keep the parent around so it can reap its children!** We do this by calling `waitpid()`.

### `pid_t waitpid(pid_t pid, int *status, int options)`
- Returns pid of child if okay, 0 (if WNOHANG) or -1 on error.
- Determining a wait set (processes the parent needs to wait for):
	- If `pid > 0`, then wait for single process identified by `pid`.
	- If `pid = -1`, then wait set consists of all child processes.
- We can modify the default behavior that occurs when `options = 0`:
	- WNOHANG: returns immediately with 0 if no members in wait set have terminated yet.
	- WUNTRACED: suspends calling process until a single process in wait set terminates or stops.
	- WNOHANG | WUNTRACED: return immediately, with a return value of 0, if none of the children in the wait set has stopped or terminated, or with a return value equal to the PID of one of the stopped or terminated children.
- **Exit status of reaped child** determined by `status` parameter:
	- WIFEXITED(`status`): true if child terminated normally (`exit` call or return)
	- WEXITSTATUS(`status`): returns exit status of a normally terminated child; WIFEXITED must return true for this to be defined.
	- WIFSIGNALED(`status`): true if child terminated because of signal that was not caught.
	- WTERMSIG(`status`): returns number of signal that caused child termination; WIFSIGNALED must return true for this to be defined.
	- WIFSTOPPED(`status`): true if child that causes return is stopped
	- WSTOPSIG(`status`): returns number of sinal that caused child to stop; WIFSTOPPED must return true for this to be defined.
- **Error conditions:**
	- If `waitpid` returns -1 and `errno` is ECHILD, calling process has no children.
	- If `waitpid` returns -1 and `errno` is EINTR, calling process was interrupted by signal.

### `unsigned int sleep(unsigned int secs)`
- Suspends a process for a specified period of time.
- Returns seconds left to sleep (0 if time elapsed).
- `int pause(void)` puts the calling function to sleep until a signal is received.

### `int execvp(const char *file, char *const argv[])`
- Loads and runs a **new program in the context of the current process**. Does NOT create a new progress.
- Returns to the calling program only if there is an error (called once and never returns).
- `argv[0]` is the name of the executable object file, and the last element in `argv[]` is NULL. If `*argv[argc - 1]` is &, then the program should run in the background.
- After `execvp()` is called, the `file` is loaded and the startup code sets up the stack, passing control to the main function of the new program.

### Using `fork` and `execvp` to Run Programs
- A **shell** is an interactive application-level program that runs other programs on behalf of the user. The original shell was the `sh` program, followed by variants like `bash`.
- The shell performs a sequence of read/evaluate steps before terminating.
- **Background** program: the shell does not wait for it to complete.
- **Foreground** program: the shell does wait for it to complete.

## Ch. 8.5: Signals
- Signals allow processes and the kernel to interrupt other processes.
- A **signal** is a small message that notifies a process that an event of some type has occurred in the system. Type `man 7 signal` to see all signals.
- CTRL-C sends SIGINT to interrupt a foreground process.
- CTRL-Z sends a SIGTSTP signal to suspend the foreground process.
- SIGKILL forcibly terminates a process.
- SIGCHLD is sent to the parent when a child process terminates or stops.
- A signal that has been sent but not yet received is called a pending signal. At one pint in time, there is at most one pending signal of a particular type. All subsequent signals of the same type are not queued. The set of pending signal is maintained in the `pending` bit vector.
- When a signal is **blocked**, the pending signal cannot be handled until it's unblocked. The set of blocked signals is maintained in the `blocked` bit vector.

### Process Groups
- Every process belongs to exactly one process group, which is identified by a proucess group ID. `pid_t getpgrp(void)` returns the pgid of the current process.
- By default, a child process belongs to the same process group as its parent. You can change the process group by using `int setpgid(pid_t pid, pid_t pgid)`.
	- Returns 0 on success, -1 on error.
	- If `pid` is 0, the pid of the current process if used.
	- If `pgid` is 0, the pid the process specified by `pid` is used as the pgid.
- `int kill(pid_t, pid, int sig)` can be used to send a signal to any process group or process. If `pid` is -1, then the signal is sent to all processes in the group.
- A **job** represents the processes that are created as a result of evaluating a single command line statement.

### Signal Handling
- Use the `sighandler_t signal(int signum, sighandler_t handler)` function to install a signal handler function that is called asynchronously, interrupting the infinite `while` loop in `main`, whenever the process receives a signal.
- Each signal type has a predefined default action:
1. The process terminates.
2. The process terminates and dumps core.
3. The process stops until restarted by a SIGCONT signal.
4. The process ignores the signal.
- These default actions can be overwritten with `signum`, which can be:
1. SIG_IGN: signals of type `signum` are ignored.
2. SIG_DFL: action for signals of type `signum` reverts to the default.
3. The address of a user defined function.

### Signal Handling Issues
- Pending signals are blocked.
- Pending signals are not queued. The key idea is that the existence of a pending signal merely indicates that at least one signal has arrived.
	- **Signals can't be used to count the occurrence of events in other processes.**
	- For instance, the SIGCHLD handler must be able to reap as many zombie children as possible each time it's invoked.
- System calls can be interrupted. Slow system calls (like `read`, etc.) need to be manually restarted so that the signal handling is portable across different systems.
- Best idea? Create a `Signal` wrapper that does the following:
	- Only signals of the type currently being processed by the handler are blocked.
	- As with all signal implementations, signals are not queued.
	- Interrupted system calls are automatically restarted whenever possible.

### Explicitly Blocking and Unblocking Signals

#### `int sigprocmask(int how, const sigset_t *set, sigset_t *oldset)`
- Changes the set of currently blocked signals.
- Values for `how`:
1. SIG_BLOCK: add signals in `set` to `blocked`
2. SIG_UNBLOCK: remove signals in `set` from `blocked`
3. SIG_SETMASK: `blocked = set`
- If `oldset` is non-NULL, the previous value of `blocked` is stored in it.
- That's where this chunk of code that appears in 110 programming assignments comes in:
    sigset_t additions, existing;
    sigemptyset(&additions);
    sigaddset(&additions, SIGCHLD);
    sigprocmask(SIG_BLOCK, &additions, &existing);
    // ... do some stuff... maybe a sigsuspend()
    sigprocmask(SIG_UNBLOCK, &additions, NULL);

### Avoiding Concurrency Bugs and Race Conditions
- Goal: synchronize concurrent flow to allow the largest set of feasible interleavings such that each interleaving produces a correct answer.

#### Race conditions:
- Consider a scenario where a parent creates a new child process, it adds the child to the job list. When the parent reaps a terminated (zombie) child in the SIGCHLD signal handler, it deletes the child from the job list.
- It is possible for `deletejob` to be called before `addjob`, if the child process completes before the parents and becomes a zombie. This is incorrect, and there is a race between the call to `addjob` and the call to `deletejob`.
- Solution? Block SIGCHLD signals before the call to `fork` and the ublock them only after we have called `addjob`. This way, the child will only be reaped after it's been added to the job list. 
- Also, children inherit the `blocked` set of their parents, so we must be careful to unblock the SIGCHLD signal in the child before calling `execvp`.
- You can expose races in your code by randomly deciding to sleep in the parent or the child. You then have a higher likelihood of exercise both orderings of child and parent executions, regardless of the particular kernel’s scheduling policy.

## Ch. 8.7: Tools for Manipulating Processes
- `strace`: prints trace of each system call invoked by a running program and its children.
- `ps`: lists processes and zombies currently in the system.
- `top`: prints info abuot the resource usage of current processes.
- `pmap`: displays the memory map of a process
