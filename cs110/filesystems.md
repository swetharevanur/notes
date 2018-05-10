# Filesystems

## Overview of Topics
- Linux and C libraries for file manipulation: `stat`, `struct stat`, `open`, `close`, `read`, `write`, `readdir`, `struct dirent`, file descriptors, regular files, directories, soft and hard links, programmatic manipulation of them, implementation of ls, cp, find, etc.
- Naming, abstraction and layering concepts in systems as a means for managing complexity, blocks, inodes, inode pointer structure, inode as abstraction over blocks, direct blocks, indirect blocks, doubly indirect blocks, design and implementation of a file system
- Additional systems examples that rely on naming, abstraction, modularity, and layering, including databases, DNS, TCP/IP, network packets, HTTP, REST, descriptors and pids
- Building modular systems with simultaneous goals of simplicity of implementation, fault tolerance, and flexibility of interactions

## What is a Filesystem?

## Questions
1. Zombie processes and reaping children
	- how is `waitpid` like `free`
	- A zombie process is when a parent that doesn't wait on its child, and the parent finishes and the child has no one to collect its resources. A parent needs to collect resources for a child.
	- This is what waitpid handles.
	- waitpid without fork always causes zombie processes
	- Reaping the children = calling waitpid
2. Masks, sigsuspend, and blocking
	- mask2 (existing) stores your original blocking state before new signals have been added through mask1 (addition)
	- sigsuspend looks for any signals that are not in mask2 (which, for our purposes, is always an empty set)
	- the reason we need to block certain signals is to avoid race conditions (when multiple processes access same global variable at the same time; e.g. joblist in stsh.cc)
3. inode vs. vnode
	- inode = thing on actual hard disk
	- vnode = third table on kernel (main memory). has pointer to inode or takes cached version of inode that it stores