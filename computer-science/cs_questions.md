## OPERATING SYSTEM
1.	What is process, thread? What are the differences between them?

a.	What data process, thread need to live? Why they said that Thread is a lightweight process?

•	A process is a program in execution. Each process is executed in separated space, and one process can’t access other process’s data. If a process wants to access other process data and resource, an inter-connection between process must be established.

•	A thread exists within the process and shares the process resources (including heap space). Each thread still has its own stack and registers. If a thread modifies data in the heap, other threads will see the update.

b.	What is multi-process and multi-thread? When we should you which one?

•	Multiprocessing is when the system has two or more processes executed at the same time by using multiple processors (CPU). 

•	Multi-thread is one process has multiple code segments (thread).

•	It depends on our system to decide which we should use. In most cases, if we have multiple processors, we should use multiprocessing and multithread at the same time.

i.	Process has how many states? How does it change between each state?

Process has 7 states: new, ready, running, waiting, terminated, suspend ready and suspend wait. 

+ New: the process is about to create but not yet created. It’s the program inside hard disk ready to be picked by OS.

+ Ready: the process is inside RAM and wait to be scheduled by OS to execute.

+ Running: The CPU is executing the process.

+ Waiting: the process requests an I/O or needs input from user or needs access to critical region (where it’s been locked).

+ Terminated: The CPU finishes executing.

+ Suspend ready: Process that was initially in the ready state but were swapped out of main memory and placed onto external storage by scheduler.

+ Suspend wait: Like suspend ready but uses the process which was performing I/O operation and lack of main memory caused them to move to secondary memory.

ii. Scheduling algorithm (Round robin, …) 

iii. What will happen if a process is waiting? Or a thread is sleeping?
     + Waiting: the process will release all the resources hold by the process and waits for other process to be executed.
    +  Sleep: the process will keep the lock on all the resources till the sleep time is over and again starts the execution of process.

iv. How CPU detects that a thread is sleeping? Or detect when it wants to run?

The scheduler will maintain a table saying that the process is waiting or sleeping.

c.	What is thread-pool? How to use it? Describe how to create a thread pool in Java.

Thread pool is used to limit the number of threads can be run at a given time. Instead of creating thread for each task, at a given time, a task will be taken to thread pool and assigned to a “free” thread.

d.	Can 2 different processes access or change data of each other address space? 

No, if no communication method (shared memory or message parsing). 

i.	Can 2 processes use the same library (for eg: libc is required to every process)? How?

Yes, we can take the library as the common resource between 2 processes.

ii.	How does debugger work? How can it attach to a running process and change data of that process?

e.	How can two processes communicate with each other?

Shared memory and message parsing.

+ Shared memory: some memory will be shared between processes (any process can access it)
 
+ Message parsing: establishes communication link and then exchange the messages.

f.	What is child-process? How to create a child-process?

A child process is a process created by a parent process in operating system using a fork() system call.

i.	What data a child-process have when we create it?

A child process will have the same data as parent process.

ii.	Can it read/write data on its parent process?

No, after using the fork() system call, two processes are independent.

iii.	What is copy on write (COW)?

When a parent process creates a child process, both will initially share the same memory space. If any processes try to modify the data, a copy of page will be created, and 
the modification will happen in the copied version.

iv.	What will happen when child-process changes a variable of parent process?

After creating, the child process and the parent process are independent, so it will not affect the variable of parent process.

v.	If file descriptor also be inherited by the child process. What if 2 processes can handle a same file descriptor or even a same socket?

2.	Concurrency and Parallels? 

Concurrency relates to processing multiple tasks at the same time, by using the context switch. Parallel means dividing task to multiple sub-tasks, and then each processor will run each sub-task (couldn’t happen if we only have one processor).

a.	What is critical zone?
Shared memory contains shared variables or resources which are needed to be synchronized to maintain consistency of data variable. Critical section contains shared variables or resources which are needed to be synchronized to maintain variable consistency.

b.	What is race condition and how to handle this case?
Race condition happens when multiple threads/processes try to access and change the variables in the shared memory. To handle this case, we need to use process synchronization.

c.	What is locking mechanism? Mutex? Semaphore? Spinlock? Read lock vs write lock?

d.	What is deadlock and how to avoid deadlock?

Deadlock is the situation where a set of processes are being blocked because each process is holding a resource and waiting for another resource acquired by some other processes. 

3.	How does memory is managed in OS?

a.	What is virtual memory? Why do we need it? How does it work?	

-	When executing a process, we maintain only some pages of processes, such memory is called virtual memory and part of processes inside RAM is called virtual address space.

-	We need virtual for 2 reasons:

+ Increase the degree of multiprogramming.

+ Even if the size of process is larger than RAM, we can still execute the process.

•	How large is virtual memory?

The size of virtual storage is limited by the addressing scheme of the computer system and the amount of secondary memory (hard disk) is available

•	What is paging?

Is the process of dividing the process into parts, usually with the same size as frame of RAM.

•	Can two processes have the same physical address? How and in which cases?

Two processes can use the same shared memory. To save physical memory space, programs using the same read-only code libraries will be using a shared single copy of the library.

b.	What is heap space and stack space?

•	Stack space: 

+ The size of memory to be allocated is known to the compiler and whenever a function is called, its variables get memory allocated on the stack.

+ After execution, the variables inside the stack will be deleted.

+ Has less storage space compared to heap.

•	Heap space:

+ The memory is allocated during the execution of instructions written by programmers.

+ When creating an object, the actual object is stored in heap, while the pointer is in stack.

+ We must delete the object after using by ourselves (to avoid the problem about memory leak).

c.	What will happen with memory when you open an application?

OS will first check the amount of memory available, and then move a copy of application (or some pages of application if we don’t have enough space) to RAM. CPU will execute the process.

d.	What will happen when you call another function with parameters or return from a function?

•	What will happen with stack? Why don’t we use heap here?

-	When we call a function, a new stack frame is created with all the function’s data and this frame is pushed into stack. Then: 

+ Sub-routine instructions are executed.

+ Stack frame is popped from the stack.

+ Now, the program counter is holding the return address.

-	We don’t use heap as we need to store the functions execution sequentially.

•	What will happened with registers?

e.	What causes stack overflow?

A stack overflow is when you use all the memory for the stack than your program was supposed to be (usually because of infinite recursion).

f.	What is dynamic allocating? How does it work?

Dynamic allocating is the process of assigning memory to process during its run time. It can be done both in stack and heap:

-	Stack: a typical example is stack when we do recursion

-	Heap: new operator. 

g.	Why don’t you need to "deallocate" local variable?

As local variables are stored inside the stack, the compiler will automatically deallocate them.

h.	How does Garbage Collection work? When it will be triggered?

(https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/)

•	To determine which objects are no longer in use, the JVM intermittently runs what is very aptly called a mark-and-sweep algorithm:

+ The algorithm traverses all object references, starting with the GC roots, and marks every object found as alive.

+ All the heap memory that is not occupied by marked objects is reclaimed. It is simply marked as free, essentially swept free of unused objects.

•	When a JVM runs out of space in the storage heap and is unable to allocate any more objects (an allocation failure), a garbage collection is triggered.

i.	What is a pointer? What difference between pass by value and pass by reference?

•	Pass by value: A copy of variable is passed to the parameter of function.

•	Pass by reference: The memory address of variable is passed to function.

j.	Where global variable will be saved?

Global and static variable will be stored in data segment.

4.	Why in Linux “Everything is file”?

5.	What is system call (sys call)?

In computing, a system call is the programmatic way in which a computer program requests a service from the kernel of the operating system it is executed on. A system call is a way for programs to interact with the operating system.

Kernel is the most important part of OS (the part that is frequently executed by CPU).
Processes can be executed in two modes: user mode and kernel mode. In kernel mode, the process can access any hardware devices and PCB. 

a.	How to do a system call?

When need, we can write code (create, open, …) so the program will know to call to OS to do the system call.

b.	What happens with CPU when we execute a syscall?

The register of CPU will change the bit mode. 

c.	What is user space and kernel space?

Memory is divided into two distinct areas:

•	The user space, which is a set of locations where normal user processes run (everything other than the kernel). The role of the kernel is to manage applications running in this space from messing with each other, and the machine.

•	The kernel space is where the code of the kernel is stored and executes under.

6.	Caching:

a.	What is in-memory cache? (memcached/redis)

b.	LRU? implement LRU in your program language! (How about multi-thread?)

(https://leetcode.com/problems/lru-cache/discuss/?currentPage=1&orderBy=hot&query=)

c.	How to migrate Cache stampede? 

d.	Quicksort(O(n^2) in worst case) vs Merge sort (O(nlogn) in worst case). Which is faster? Why? How they use these 2 sorting algorithms in real life?
-	https://www.geeksforgeeks.org/quick-sort-vs-merge-sort/




7. Recommend resource: Personally, I don't study computer science at college. So this sequence on Udemy would be a good resource to study operating system for anyone who comes from different background like me: https://www.udemy.com/course/operating-systems-from-scratch-part1/ . My advice would be study all 4 parts to get the foundation for operating system, and then try to answer the questions here. 



