## 12 A Dialogue on Memory Virtualization
> every address generated
by a user program is a virtual address.

## 13 Address Spaces

### 13.1 Early Systems
早期的机器没有提供太多的抽象,OS和program存放在物理内存上.

### 13.2 Multiprogramming and Time Sharing
为了使time sharing更加高效,更加快.我们把几个想要运行的process都存放在memory中.这样又产生了一个问题,如何防止process之间读取信息(恶意的)?

### 13.3 The Address Space
我们把物理内存抽象成**address space**.

> The address space of a process contains all of the memory state of the
running program

隔离性原则(THE PRINCIPLE OF ISOLATION)是构造可靠系统的关键性原则.

### 13.4 Goals
* One major goal of a virtual memory (VM) system is **transparency**.即程序不应该意识到内存被虚拟化了.
* Another goal of VM is **efficiency**.OS应该使虚拟化尽可能的有效率.
* Finally, a third VM goal is **protection**.OS应该保护一个process不被别的process所干扰,当然对于OS自身也是一样.这样我们就引出了一个性质:**isolation**.

你看到的所有地址都是虚拟的.*if you
print out an address in a program, it’s a virtual one, an illusion of how
things are laid out in memory; only the OS (and the hardware) knows the
real truth.*

### 13.5 Summary
> The VM system is responsible for providing the illusion of a large,
sparse, private address space to programs, which hold all of their instructions
and data therein.

## 14 Memory API
如何分配与管理内存?

### 14.1 Types of Memory
运行C程序时,会分配两种内存.一种是**stack**内存,由编译器隐式地分配.第二种是**heap**内存,由我们自己隐式地分配.

### 14.2 The malloc() Call
> The malloc() call is quite simple: you pass it a size asking for some
room on the heap, and it either succeeds and gives you back a pointer to
the newly-allocated space, or fails and returns NULL
.

### 14.3 The free() Call
调用free()去释放不再使用的堆内存,*The routine takes one argument, a pointer that was returned by malloc().*

### 14.4 Common Errors
调用malloc()和free()有很多常见的错误.
* Forgetting To Allocate Memory
* Not Allocating Enough Memory
* Forgetting to Initialize Allocated Memory
* Forgetting To Free Memory
* Freeing Memory Before You Are Done With It
* Freeing Memory Repeatedly
* Calling free() Incorrectly

### 14.5 Underlying OS Support
malloc()和free()是基于system call的.例如brk或者sbrk,但是要注意不要去亲自调用他们.

### 14.6 Other Calls
> realloc() makes a new larger region of memory,
copies the old region into it, and returns the pointer to the new region.

## 15 Address Translation
为了CPU的虚拟化,我们关注一种一般化的机制:**limited direct execution (or LDE)**.

在进行内存虚拟化时,我们的主要策略还是在实现想要的虚拟化效果的同时高效和可控.

如何有效而且灵活的内存虚拟化?

一种通用的技术:**hardware-based address translation**简单来说是**address translation**.

当然只有硬件不能完成内存虚拟化,OS还需要**manage memory**,*keeping track of which
locations are free and which are in use, and judiciously intervening to
maintain control over how memory is used*.

总之我们想要的是一个美丽的幻象:*that the program has its own private memory, where its own code
and data reside*.然而实际上:*that many programs are actually sharing memory at the same time, as
the CPU (or CPUs) switches between running one program and the next*.

### 15.1 Assumptions
就目前来说我们会做以下假设:
* we will assume for now that the user’s address space must be placed **contiguously** in physical memory. 
* We will also assume, for simplicity,
that the size of the address space is not too big; specifically, that
it is **less than the size of physical memory**. 
* Finally, we will also assume that
each address space is exactly the **same size**.

### 15.2 An Example
> Interposition is a generic and powerful technique that is often used to
great effect in computer systems.

### 15.3 Dynamic (Hardware-based) Relocation
为了更加理解hardware-based address translation,我们介绍第一个思想:**base and bounds**也被称为**dynamic relocation**.

> we’ll need two hardware registers within each CPU: one
is called the base register, and the other the bounds (sometimes called a
limitregister). This base-and-bounds pair is going to allow us to place the
address space anywhere we’d like in physical memory, and do so while
ensuring that the process can only access its own address space.

Sometimes people call the
part of the processor that helps with address translation the **memory
management unit (MMU)**

### 15.4 Hardware Support: A Summary
总结一下我们需要哪些硬件支持

### 15.5 Operating System Issues
为了base-and-bounds version
of virtual memory,操作系统也有新的问题需要解决.
* First, the OS must take action when a process is created, finding space
for its address space in memory.
* Second, the OS must do some work when a process is terminated (i.e.,
when it exits gracefully, or is forcefully killed because it misbehaved),
reclaiming all of its memory for use in other processes or the OS. 
* Third, the OS must also perform a few additional steps when a context
switch occurs.
* Fourth, the OS must provide exception handlers, or functions to be
called, as discussed above; the OS installs these handlers at boot time (via
privileged instructions).

### 15.6 Summary
> With address translation, the OS can control each and
every memory access from a process, ensuring the accesses stay within
the bounds of the address space.

保持高效的关键.

Base-and-bounds virtualization的优点.

不幸的是,这种简单的动态分配技术也有缺点,造成空间的浪费被称为**internal fragmentation**.

## 16 Segmentation
base and bounds没有我们想象中的灵活.我们要如何避免一个大的地址空间中stack和heap之间有很多没有利用的空间?

### 16.1 Segmentation: Generalized Base/Bounds
**segmentation**可以解决上述问题

> The idea is simple: instead of having just one base
and bounds pair in our MMU, why not have a base and bounds pair per
logical segment of the address space?

在我们的例子中,我们有三个**logically-different segments**:code, stack,
and heap.那么运用分割的思想,我们独立地放置每个部分来避免没有使用的虚拟地址空间占满物理内存.

当引用一个不合法的地址,或者引用的地址超出所分配的空间时会引起**segmentation fault**也即**segmentation violation**.

### 16.2 Which Segment Are We Referring To?
> One common approach, sometimes referred to as an explicit approach,
is to chop up the address space into segments based on the top few bits
of the virtual address.

> There are other ways for the hardware to determine which segment
a particular address is in. In the implicit approach, the hardware determines
the segment by noticing how the address was formed.

### 16.3 What About The Stack?
在此之前,我们都没有提到stack的地址空间,Stack有一个特殊的特性:*it grows backwards*他的地址是向上增加的.因此translation必须有些不同.

为此,我们需要一些硬件支持,硬件必须知道segment增长的方向.

### 16.4 Support for Sharing
> Specifically, to save memory, sometimes it is useful to share
certain memory segments between address spaces.

为了实现共享,我们需要一些硬件的支持,通过**protection bits**的方式:*Basic support adds a few bits per segment,
indicating whether or not a program can read or write a segment, or perhaps
execute code that lies within the segment.*

### 16.5 Fine-grained vs. Coarse-grained Segmentation
我们之前举的例子,都是把内存分为一些部分(i.e., code, stack, heap)这样被称为**coarse-grained**.而在一些早期系统,内存被分为很多小的部分,这样的被称为**fine-grained**.

支持更多的segment需要更多的系统支持,例如**segment table**或类似的东西存放在内存中.

### 16.6 OS Support
分段带来了一些新的问题,我们先来描述一些待解决的OS问题:
* The first is an old one:
what should the OS do on a context switch?
* The second, and more important, issue is managing free space in physical
memory.

一个问题是当出现很多小的细碎的空间时,我们很难去分配新的segment和去增长已经存在的segment,我们称这个问题为**external fragmentation**.

解决这个问题的一个办法是重新分配已存在的segment,来压紧物理内存.

还有一种更简单的方法是*use a free-list management algorithm that
tries to keep large extents of memory available for allocation*.例如**best-fit**,**worst-fit**,**first-fit**或者一些更复杂的方案**buddy algorithm**.

### 16.7 Summary
这一章最关键的就在于segmentation.

## 17 Free Space Management 
如何管理空闲空间?

### 17.1 Assumptions
* We assume a basic interface such as that provided by *malloc()* and
*free()*.
* We further assume that primarily we are concerned with **external fragmentation**.
* We’ll also assume that once memory is handed out to a client, it cannot
be relocated to another location in memory.
* Finally, we’ll assume that the allocator manages a contiguous region
of bytes.

### 17.2 Low-level Mechanisms
#### Splitting and Coalescing
是一个在所有allocator间通用的技术.

#### Tracking The Size Of Allocated Regions
free()接口不需要接受参数,解释在原文中.

#### Embedding A Free List
#### Growing The Heap
当heap的空间溢出了怎么办?最简单的方法就是报错!

### 17.3 Basic Strategies
每种策略都有其适用条件,在这里介绍一些基本的策略和优缺点
* Best Fit
* Worst Fit
* First Fit
* Next Fit

### 17.4 Other Approaches
除了上面那些基础方法,还有很多被推荐的技术和算法可以去改进内存分配.
* Segregated Lists
* Buddy Allocation

### 17.5 Summary
> In this chapter, we’ve discussed the most rudimentary forms of memory
allocators.

## 18 Introduction to Paging
虚拟内存空间管理的问题,把一整块空间分割成几块固定大小的空间的思想被称为**paging**.

这一次,我们不按logical segments分,而是直接分为固定大小的单元,每个单元称为**page**.我们把物理内存看做一个队列称为**page frames**.

### 18.1 A Simple Example And Overview
