# [Bryant & O'Hallaron, Ch. 10](http://www.codeman.net/wp-content/uploads/2011/12/Computer_Systems-A_Programmers_Perspective-2e.pdf#page=896): System-Level I/O

## Ch. 10.1: Unix I/O
- I/O = copying data from RAM (input) and to external devices like hard drives, etc. (output).
- For our purposes, **kernel = main memory**.
- All devices are modeled as files.

### Opening Files
- To access a device, `open` a file. Returns a small nonnegative integer, known as a **descriptor**. The file descriptor is used to keep track of all the file information, so the application only needs to keep track of the descriptor.
- Each process starts with 3 open files: standard input (descriptor 0; STDIN_FILENO), standard output (desc. 1; STDOUT_FILENO), and standard error (desc. 2; STDERR_FILENO).

### File Position
- For each open file, we need a **file position** $k$ that is a byte offset from the start of the file. 
- Initially $k = 0$. 
- We can set $k$ with `seek`.

### Reading and Writing Files
- `read` starts at $k$, reads $n$ bytes, then increments $k$ by $n$.
- If $k \geq numTotalBytesInFile$, we reach end-of-file.
- `write` copies $n$ bytes from memory to a file starting at $k$, then increments $k$ by $n$.

### Closing Files
- For when an application has finished accessing a file
- `close` tells the kernel to free the data structures it created when the file was open, and restoring the descriptor to the pool of free descriptors. 
- Called when a process terminates for any reason.

## Ch. 10.2: Opening and Closing Files

### `int open(char *filename, int flags, mode_t mode)`
- **Converts filename to a file descriptor and returns the descriptor number** (or -1 on error).
- Always returns smallest possible descriptor. 
- There are three flags: 
1. O_RDONLY (read)
2. O_WRONLY (write)
3. O_RDWR (both)
- You can or these flags with other bit masks too:
1. O_CREAT (create new file if doesn't exist)
2. O_TRUNC (if file exists, truncate it)
3. O_APPEND (before each write operation, set $k$ to EOF
- The third argument mode tells you R/W/X permissions.
- Creates an entry in the file entry table.

### `int close(int fd)`
- 0 if success, -1 if error
- Closing a descriptor that is already closed is an error!

## Ch. 10.3: Reading and Writing Files

### `ssize_t read(int fd, void *buf, size_t n)`
### `ssize_t write(int fd, const void *buf, size_t n)`

- `size_t` = `unsigned int`
- `ssize_t` = `int`
- Short counts = when the function transfers fewer bytes than the application requests. Why?
1. Encountering EOF.
2. Reading lines of text from a terminal.
3. Reading and writing network sockets.
- Handle short counts by repeatedly calling `read` and `write` until all requested bytes have been transferred.

## Ch. 10.5: Reading File Metadata
- You can retrieve file metadata with `stat` and `fstat`.
- Both return 0 on success, -1 if error.

### `int stat(const char *filename, struct stat *buf)`
- Populates members of 	`stat` struct.
![](https://image.ibb.co/cq2VRJ/Screen_Shot_2018_05_10_at_1_53_39_PM.png)
- The `st_mode` bits help us determine file type (and also encodes permissions).
1. `S_ISREG(stat.st_mode)`: is this a regular file (binary or text data)?
2. `S_ISDIR(stat.st_mode)`: is this a directory file?
3. `S_ISSOCK(stat.st_mode)`: is this a network socket?
- The `st_size` member contains file size in bits

### `int fstat(int fd, struct stat *buf)`
- Same as `stat` but takes in file descriptor instead of name.

## Ch. 10.6: Sharing Files
- Kernel opens files using three data structures:
	1. File descriptor table
		- **Each process has its own FD table.**
		- Each open descriptor points to an entry in the file entry table.
2. File entry table
- Represents the set of open files **shared by all processes**.
- Each entry has:
	- Current file position
	- **Reference count** of the number of descriptors pointing to it
	- **Pointer to an entry in the v-node table**
- Closing a file descriptor decrements the reference count.
- Once reference count hits 0, kernel deletes that file entry and associate v-node entry.
3. v-node table
- Shared by all processes.
- Contains a lot of the information in the `stat` struct.

- **File sharing:** Multiple descriptors can reference the same file (i.e. the same vnode entry) through different file table entries.
	- Can happen if you call `open` twice with the same filename.
	- Gives each descriptor the ability to fetch data from unique file positions.

- **Child processes:** Inherit parent's open files. So children have their own FD tables that point to the same file entry table locations.
	- Induced by `fork()` calls.
	- Parent and child must both close their descriptors now.

## Ch. 10.7: I/O Redirection
- I/O redirection operators allow users to associate standard input and output with disk files.
- Similar to what a Web server does when it runs a CGI (common gateway interface) program. 

### `int dup2(int oldfd, int newfd)`
- Returns nonnegative descriptor on success, -1 if error.
- Copies descriptor table entry `oldfd` into `newfd`, overwriting whatever was in `newfd` before (i.e. closing `newfd` first.
- When you say redirect A to B, the syntax is `dup2(B, A)`.

## Ch. 10.8: Standard I/O
- C has a higher-level alternative to Unix I/O.
- The standard C I/O library models an open file as a stream that is a pointer to a `FILE` struct. `FILE` is an abstraction for a file descriptor. 
- Every program begins with three open streams: `stdin`, `stdout`, and `stderr` (which correspond to our three file descriptors).