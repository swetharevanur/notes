# Miscellaneous Multiprocessing Notes (written referencing [this](https://github.com/Leedehai/CS110Notes/blob/master/CS110NotesCollection/Topic%202%20Multiprocessing%20(2).md) and [this](https://github.com/Leedehai/CS110Notes/blob/master/CS110NotesCollection/Topic%202%20Multiprocessing%20(3).md))

## Main Calls
- `fork()` replicates the parent to a new child process, and returns to child's PID to the parent
- `waitpid()` blocks the parent and waits on the child specified by its PID
- `execvp()` replaces the entire memory space of the parent with the child's context, but does not change the process's PID and parent-child relationship

## Inter-Process Communication

### Pipes
- A **pipe** is a one-way connection that carries a byte stream from one file (i.e. one file descriptor) to another.

#### `pipe()`
- Takes in an integer array of length two, and fills the array with two new file descriptors. Now, the second file can send a byte stream to the first.
- Returns 0 on success, -1 otherwise (`errno` is also set).
- With `fork()` and `pipe()`, we can establish a channel for communication between parent and child processes. 
- **Corresponding file descriptors in the parent and the child process point to the same file via a shared entry in the system-wide file entry table. So if the parent or the child writes to a sending file, both the parent and child can receive the data from the receiving file. So with `fork()`, there are 4 channel established.**
- Once data is read from the receiving end, it can't be read again from another receiving end. 

### Redirection with `dup2()`
- Takes in two file descriptors. Disocciates the second FD from its corresponding file, and redirects it to the file pointed to by the first FD.

## Signals

- A process can selectively block certain signals. When a signal type is blocked, any signals of this type won't be received by the process until it's unblocked.
- Blocking certain signals **forces an execution order** of the parent and child processes. Without blocking, the order of process execution is not guaranteed.
- You can block and unblock signals with `sigprocmask()`. See bo_ch8.md for more details.
- The system call `raise()` can send a signal to the calling process itself, taking the signal value as its sole argument.
- The system call `sigsuspend()` can temporarily override signal block and suspend the calling process to wait for the intended signal(s) specified in its argument (of type `sigset_t`).

## Scheduling
- There are three classes of processes: running, runnable, and blocked.
- **Each CPU core can handle one process at a time.**
- A process spends most of its lifetime on the runnable queue.
- The blocked set includes those that are blocked by `sleep()`, `waitpid()`, `sigsuspend()`, etc.