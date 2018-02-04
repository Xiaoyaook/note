# 	Virtualization

## processes
processes的定义:
> The definition of a process, informally,
is quite simple: it is a running program

*HOW TO PROVIDE THE ILLUSION OF MANY CPUS?*

通过虚拟化CPU来解决此问题,用到了**time
sharing**机制

> To implement virtualization of the CPU, and to implement it well,
the OS will need both some low-level machinery as well as some highlevel
intelligence.

我们称low-level machinery为**mechanisms**，称highlevel intelligence为**policies**。

### The Abstraction: A Process
去理解一个process的构成，我们需要理解其**machine state**。

machine state的一个显而易见的成分是**memory**。
> the memory that the
process can address (called its address space) is part of the process.

另一个machine state的成分是**registers**。

### 4.2 Process API
这些API基本适用于任何现代操作系统：
* Create：创建新的process
* Destroy：强制销毁process
* Wait: 等待一个process停止运行
* Miscellaneous Control: 其他对于process的控制方式
* Status: 获取process的状态信息

### 4.3 Process Creation: A Little More Detail
程序如何被转化为进程的。

*Specifically, how does the OS get a program up
and running? How does process creation actually work?*

运行一个程序，首先要加载其代码和所有静态资源到内存中,到这个进程的地址空间去。*在早期操作系统这个过程是**eagerly**的，在现代操作系统中这个过程是**lazily**的。真正理解懒加载需要了解**paging and swapping**机制.*

当加载完毕时，一些内存需要分配给程序的堆和栈.

操作系统还会做一些其他的初始化工作，特别是与IO连接。

最后启动程序，操作系统把控制权从CPU转移到新创建的进程。

### 4.4 Process States
简单的来说，一个process有三个状态：
* Running: 一个process在处理器上运行。
* Ready: 一个process已经准备好运行，但还没有运行。
* Blocked: 还不能运行，直到一些事件发生。

除了这三个以外还可能有其他的状态，比如**initial**或**final**

### 4.5 Data Structures

操作系统有很多关键的数据结构,去追踪各种各样无关的消息。比如**process list**，还有其他的数据结构之后将会看到。

### 4.6 Summary
> the low-level
mechanisms needed to implement processes, and the higher-level policies
required to schedule them in an intelligent way. By combining mechanisms
and policies, we will build up our understanding of how an operating
system virtualizes the CPU.

## Process API
讨论Unix系统中process的创建,Unix提供创建一个新的process的方式,调用**fork()**和**exec()**。还有一个方法**wait()**,可用于一个process等待一个process结束。

### 5.1 The fork() System Call
> The fork() system call is used to create a new process

### 5.2 The wait() System Call
>　it is quite useful for a
parent to wait for a child process to finish what it has been doing. This
task is accomplished with the wait() system call

### 5.3 Finally, The exec() System Call
> This system call is useful when you want to run a program
that is different from the calling program.

exec()并不创建一个新的process，而是将现在运行的程序转换为一个不同的程序

### 5.4 Why? Motivating The API
> fork() and exec() is
essential in building a UNIX shell, because it lets the shell run code after
the call to fork() but before the call to exec()

> this code can alter the
environment of the about-to-be-run program, and thus enables a variety
of interesting features to be readily built.

总之fork()/exec()是一种强力的方法去创建和操作processes。

### 5.5 Other Parts Of The API
除了fork(), exec(), and wait()还有很多其他与process有关的API，在此不再列举

## 6 Mechanism: Limited Direct Execution
创建**virtualization machinery**时，有几点挑战，一是**performance**，二是**control**。在可控的情况下，保持高性能，是创建操作系统的核心挑战。

### 6.1 Basic Technique: Limited Direct Execution
> The “direct execution” part of the idea is simple: just run the
program directly on the CPU.

这种直接执行的方法，会引起下述一些问题。

### 6.2 Problem #1: Restricted Operations
一种**processor mode**是**user mode**，在user mode下代码的运行会受到一些限制。

与user mode 相对的是**kernel mode**，在这种模式下可以执行任何代码。

> In user mode, applications do not have full access to hardware resources.
In kernel mode, the OS has access to the full resources of the machine.

在user mode下，想执行一些限制操作就需要用到**system call**。

执行system call时，一个程序必须执行一个特殊的**trap**指令。程序执行完毕后，再执行一个**return-from-trap**指令。

内核必须严格控制那些代码可以在**trap**下执行，所以在开机时设置一个**trap table**。

limited direct execution (LDE) protocol有两个阶段，一个是在启动时，一个是在运行process时。

### 6.3 Problem #2: Switching Between Processes
如果一个process在CPU上运行，意味着OS并不在CPU上运行。如何使OS重新获得CPU的控制权，从而切换processes呢？

#### A Cooperative Approach: Wait For System Calls
> in a cooperative scheduling system, the OS regains control of
the CPU by waiting for a system call or an illegal operation of some kind
to take place.

#### A Non-Cooperative Approach: The OS Takes Control
> The addition of a timer interrupt gives the OS the ability to run again
on a CPU even if processes act in a non-cooperative fashion. Thus, this
hardware feature is essential in helping the OS maintain control of the
machine

#### Saving and Restoring Context
上下文切换在概念上是很简单的
> all the OS has to do is save a few register values
for the currently-executing process (onto its kernel stack, for example)
and restore a few for the soon-to-be-executing process (from its kernel
stack)

注意saves发生在**timer interrupt**被触发，restores发生在OS准备从process A切换到process B时。

### 6.4 Worried About Concurrency?
当正在处理一个interrupt时，另外一个interrupt发生了，这种情况怎么办。
> One simple thing an OS might do is disable
interrupts during interrupt processing; doing so ensures that when
one interrupt is being handled, no other one will be delivered to the CPU.

当然这种方法并不好，还有一种方法是
> Operating systems also have developed a number of sophisticated
locking schemes to protect concurrent access to internal data structures.

## 7 CPU Scheduling
OS高层次的policy,调度器的应用,如何发展调度策略.

### 7.1 Workload Assumptions
>  Determining the
workload is a critical part of building policies, and the more you know
about workload, the more fine-tuned your policy can be.

关于process,或者有时被称为jobs,我们将作出以下假设:
1. Each job runs for the same amount of time.
2. All jobs arrive at the same time.
3. Once started, each job runs to completion.
4. All jobs only use the CPU (i.e., they perform no I/O)
5. The run-time of each job is known.

现在这些假设还有些不切实际,但我们可以通过这些假设管中窥豹.

### 7.2 Scheduling Metrics
除了工作量假设,我们还需要一样东西使我们能够比较不同的调度策略:调度指标.

就目前来说,我们先简单的使用一个指标: **turnaround time**.

> The turnaround time of a job is defined
as the time at which the job completes minus the time at which the job
arrived in the system.

我们还能够注意到turnaround time是一个性能指标.除了要注意**performance**指标外,我们还要注意**fairness**指标.

### 7.3 First In, First Out (FIFO)
> FIFO has a number of positive properties: it is clearly simple and thus
easy to implement. And, given our assumptions, it works pretty well.

然而FIFO并不总是有优势的,他的优势建立于我们之前的假设,即所有jobs都运行同样长的时间,这样**average turnaround time**看起来还差不多.当这个假设不成立时**average turnaround time**就有可能很难看.

这个问题被称为**convoy effect**.

### 7.4 Shortest Job First (SJF)
用时最短的jobs先执行,这样解决了turnaround time的问题.但注意我们刚才的假设2: *All jobs arrive at the same time*.

如果该条件不成立那么又会产生很多问题.

### 7.5 Shortest Time-to-Completion First (STCF)
为了解决上面那个假设2不成立的情况,我们先令假设3也不成立: *Once started, each job runs to completion*.

> add preemption
to SJF, known as the Shortest Time-to-Completion First (STCF) or
Preemptive Shortest Job First (PSJF) scheduler [CK68].

当有新任务进入队列时,STCF调度器会检查哪个任务具有最短的完成时间,并去执行那个任务.

### 7.6 A New Metric: Response Time
对于早期的**batch computing systems**来说,上述的策略已经很完善了.但对于现在的**time-shared machines**来说,一个新的需要考虑的指标产生了:**response time**.

> We define response time as the time from when the job arrives in a
system to the first time it is scheduled

### 7.7 Round Robin
STCF对于response time的表现可能不会太好,我们引入一个新的调度算法来优化response time.

> **Round-Robin (RR) scheduling**:: instead of running jobs to completion, RR runs a job for a
time slice (sometimes called a scheduling quantum) and then switches
to the next job in the run queue.

时间切片的长度对于RR来说是至关重要的,但是过短的切片也会造成问题.我们需要:*making it long enough to amortize the cost of switching without
making it so long that the system is no longer responsive.*

RR对于response time表现不错,但对于average turnaround time又表现不佳,所以具体的调度策略的选择需要根据具体的情况来选择.

### 7.8 Incorporating I/O
现在我们来解决假设4,*All jobs only use the CPU (i.e., they perform no I/O)*.

面对IO所需要的时间,我们使用**overlap**策略,当CPU执行任务等待IO这段空闲时间,转而执行另外的任务,充分利用CPU.

> While those interactive jobs are performing
I/O, other CPU-intensive jobs run, thus better utilizing the processor.

### 7.9 No More Oracle
将在之后的章节解决假设5,*The run-time of each job is known.*

> building a scheduler that
uses the recent past to predict the future. This scheduler is known as the
multi-level feedback queue

## 8 Multi-level Feedback
在这章介绍一个最为人所知的调度方法:**Multi-level Feedback
Queue (MLFQ)**.

MLFQ想要解决两个方面的问题.一是优化turnaround time,二是减小response time.

### 8.1 MLFQ: Basic Rules
> In our treatment, the MLFQ has a number of distinct queues, each
assigned a different priority level.

两个MLFQ基本规则:
* Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t).
* Rule 2: If Priority(A) = Priority(B), A & B run in RR.

那么如何设置优先级就是问题的关键,
>Rather than giving a fixed priority to each job, MLFQ varies
the priority of a job based on its observed behavior.

### 8.2 Attempt #1: How To Change Priority
优先级调整算法的一些要求:
* Rule 3: When a job enters the system, it is placed at the highest
priority (the topmost queue).
* Rule 4a: If a job uses up an entire time slice while running, its priority
is reduced (i.e., it moves down one queue).
* Rule 4b: If a job gives up the CPU before the time slice is up, it stays
at the same priority level.

#### Example 1: A Single Long-Running Job
#### Example 2: Along Came A Short Job
#### Example 3: What About I/O?
#### Problems With Our Current MLFQ
第一个问题是**starvation**:*if there are “too many” interactive
jobs in the system, they will combine to consume all CPU time,
and thus long-running jobs will never receive any CPU time (they starve).*

第二点是*a smart user could rewrite their program to game the scheduler*.

### 8.3 Attempt #2: The Priority Boost
如何避免starvation?
> The simple idea here is to periodically boost the priority of all the jobs
in system.

因此,我们再添加一条规则:*Rule 5: After some time period S, move all the jobs in the system
to the topmost queue.*

这条规则所带来的问题是,S的大小如何决定

### 8.4 Attempt #3: Better Accounting
我们把规则4a,4b重写为一条规则:*Rule 4: Once a job uses up its time allotment at a given level (regardless
of how many times it has given up the CPU), its priority is
reduced (i.e., it moves down one queue).*

> how to prevent gaming of our scheduler?

The solution here is to perform better accounting of CPU time at each
level of the MLFQ.

### 8.5 Tuning MLFQ And Other Issues



