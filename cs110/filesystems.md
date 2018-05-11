# File Systems (written referencing [this](https://github.com/Leedehai/CS110Notes/blob/master/CS110NotesCollection/Topic%201%20Filesystem%20(1).md) and [this](https://github.com/Leedehai/CS110Notes/blob/master/CS110NotesCollection/Topic%201%20Filesystem%20(2).md))

## What is a File System?
### Hard Drives and Blocks
- An storage device can be a magnetic tape, a hard disk drive, or a solid state drive.
- It is divided up into **blocks** (same thing as sectors for our purposes) of a fixed size. Technically, a block is a software abstraction of a physical sector in the device.
- These blocks names are zero-indexed.
- A block is the smallest unit for read and write operations. 
- A file might use multiple blocks (not necessarily contiguous), but a block is occupied by at most one file. 
- This is how the blocks are arranged:

![](https://image.ibb.co/kpMN6J/Screen_Shot_2018_05_11_at_12_47_53_AM.png)

- The boot block stores a program that starts the OS.
- The super block (whose name is 1) serves as an anchor the information that keeps track of which blocks are free and taken.
- **Indirect blocks** don't contain payload data, but more block numbers. This is useful for large files. 
- A directory is also a file whose payload data is a map from file names to inode numbers

### Inodes
#### What is stored in an inode?

- Owner
- File type
- Permissions
- Payload size (not including inode)
- Create time
- (Up to 8) block numbers that are storing file's payload

#### Need to know:

- Every file has exactly one inode.
- An inode is used as a container to record which blocks belong to each file.
- An inode has a fixed size.
- Stored in a dedicated part of the disk called the **inode table**.
- A block can contain multiple inodes. 
- The "i" in "inode" stands for index.
- The file's name is not stored in its inode, but in it's parent directory file's payload.
- You can examine an inode's content using the `stat` command.

When you call `open` on a file, the kernel grants the user process access to the file and then returns a file descriptor. What does that mean? Let's talk about the three tables (from the lowest level to the highest level of abstraction):

### Vnode Table
Once the kernel finds the file's inode on disk, it will cache the metadata to the **vnode table**, which is maintained in memory system-wide. 

- The "v" in "vnode" stands for virtual, as in "virtual file system". 
- There is one vnode per inode.
- Inodes are stored on the hard drive. vnodes are stored in memory and maintained by the OS kernel. vnodes are created lazily (i.e. vnode is only kept if file is in use).
- The reference count for a vnode represents the number of file entries referencing that vnode.
- `stdin`, `stdout`, and `stderr` don't have inodes on the hard drive; they only exist as vnodes.

### File Entry Table
An entry in another table, called the **file entry table** (also maintained system-wide in memory), points to this vnode entry.
 
- The file entry stores the file's permissions, the cursor offset, and the file entry reference count.
- If both the parent and child processes read from the same file, the cursor will be at 2.

### File Descriptor Table
Finally, an entry in the **file descriptor table** (maintained per-process in memory) is allocated and points to the file entry in the file entry table.

- File descriptors 0, 1, and 2 are `stdin`, `stdout`, and `stderr` streams.

> Aside:
> A symbolic link (aka symlink or soft link) is the nickname for a file. It contains a reference to another file or directory in the form or an absolute or relative path. Symlinks operate as if they are working directly on the target file. But the existence of symlinks changes an otherwise hierarchical file system into a directed graph.