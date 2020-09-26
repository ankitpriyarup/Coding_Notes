# OS

## Introduction

* A program that acts as an intermediary b/w a user of a computer and the hardware.
* Direct hardware access is neither simple nor convenient \(using machine language\) that's why OS interface \(CLI, GUI\)
* Resource Manager: Manages system resources in an unbiased manner also resolves conflicting requests.
* Provides a platform on which other application programs are installed.
* Efficient utilization of computer hardware
* Protection and Security: Protection is all access to system resource is controlled. Security of the system from outsiders by requiring user authentication.
* One program running all the time in computer is kernel others are either system or user program.
* Kernel is a core part, it acts like a bridge between applications and hardwares.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Operating System</th>
      <th style="text-align:left">Kernel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <ol>
          <li>OS is a system software</li>
          <li>OS provides interface between user and hardware</li>
          <li>It also provides protection and security.</li>
          <li>All system needs OS to run</li>
          <li>Types: Single user, multi user, Multi processor, Realtime, Distributed</li>
          <li>It is the first program to load when the computer boots up.</li>
        </ol>
      </td>
      <td style="text-align:left">
        <ol>
          <li>Kernel is a system software which is a part of OS</li>
          <li>Kernel provides interface between application and hardware.</li>
          <li>It&apos;s main purpose is memory, disk, process and task management</li>
          <li>All OS needs kernel to run</li>
          <li>Types: Monolithic, Micro</li>
          <li>It is the first program to load when OS runs.</li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>

Bootstrap program is loaded at power up or reboot. Typically stored in ROM generally known as firmware. Initializes all aspects of system - CPU registers, device controllers, memory contents, Loads OS kernel and starts execution. BIOS \(Basic Input Output System\) and UEFI \(Unified Extensible Firmware Interface\)

## Computer System Organization

### Computer System Operation

* One or more CPU devices controllers connected through common bus providing access to shared memory
* Concurrent execution of CPUs and device competing for memory cycles
* I/O devices and CPU can execute concurrently
* Each device controller is in charge of particular device type.
* Each device controller has a local buffer.
* CPU moves data from/to main memory to/from local buffer.
* I/O is from the devices to local buffer of controller
* Device controller informs CPU that it has finished its operation by causing an interrupt.

### Interrupts

* Occurrence of an event is signaled from either - Hardware \(send signal via system bus to CPU\) or Software \(by invoking system call\)
* Interrupt architecture must save the address of the interrupted instructions.
* Incoming interrupts are distributed while another interrupt is being processed to prevent a lost interrupt.

> If an asynchronous interrupt is triggered, it means that the processor will \(most likely at the next clock cycle\) save its current executing environment, and service the interrupt request. This is an example of a hardware interrupt \(one that is triggered by an external connection to the processor\).  
> Hardware device signals a need for 'attention' via interrupt request line.  
>   
> Software generated interrupt is called trap. Like zero by zero or invalid memory.

### Interrupt Handling

* The OS preserves the state of the CPU by storing registers and program counters
* Determines which type of interrupt has occured:
  * Polling \(Microcontroller keeps polling/checking devices for pending requests\)
  * Vectored interrupt system \(Devices directs to the microcontroller appropriate service routine\)

### Storage Structure

<table>
  <thead>
    <tr>
      <th style="text-align:left">Main Memory (RAM)</th>
      <th style="text-align:left">Secondary Memory</th>
      <th style="text-align:left">Magnetic Disks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <ul>
          <li>Contains programs to execute</li>
          <li>Too small for storage of all programs permanently</li>
          <li>Volatile Storage</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Extensive</li>
          <li>Backup purpose</li>
          <li>Non-volatile</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Rigid metal or glass platters covered with magnetic recorded material.</li>
          <li>Disk surface is logically divided into tracks which are sub divided into
            sectors.</li>
          <li>The disk controller determines the logical interaction b/w the device
            and computer</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

> Cache Memory, Registers

Device controller transfers entire block of data directly to/from its buffer storage to memory with no intervention from CPU. DMA \(Direct Memory Access\)

## Types of OS

### Batch OS:

* Does not interact with computers directly.
* A operator takes similar jobs having same requirements and groups them into batches.
* It is the job of the operator to sort the jobs with similar needs.
* Advantages:
  * Processor of the batch system knows how long the job would be when it is in the queue
  * Multiple users can share the batch system
  * Idle time is very less
  * It is easy to manage large work repeatedly in batch system
* Disadvantages:
  * Hard to debug
  * If a job fails other job has to wait for an unknown amount of time.
* Eg: Bank statements, Payroll system, etc.

### Time sharing / Multi tasking OS:

* Each task is given some time to execute so that all the tasks work smoothly.
* Multitasking is multi programming with time sharing.
* Multi programming - maximizes CPU utilization, more than one process ready to execute in main memory, context switch during a program idle/IO task, CPU never idle unless there's no process ready to execute. **Adv:** High CPU utilization, less waiting response time, can be extended to multiple users, Nowadays very useful. **Dis adv:** Difficult scheduling, main memory management is reqd, memory fragmentation, paging \(non-contiguous\) memory allocation

### Multi processing OS:

* 2 or more CPU within a single computer inclosed communication sharing the system bus. memory and other IO.
* Different process may run on different CPU, true parallel execution.
* Symmetric: CPUs are identical and they share main memory. Assymetric: CPUs are not identical and they follow master-slave relationship Assymetric is easy to design but less efficient.
* Adv: higher throughput, higher reliability, cost saving, battery saving, true parallel processing. Dis adv: More complex, Overhead on coupling, Large main memory.

### Distributed OS:

* Various autonomous interconnected computers communicates each other using a shared communication network.
* Independent system processes their own memory unit and CPU.
* Also refereed as loosely coupled system.
* Adv: Failure of one will not affect other as all are independent, Load on host reduces delay in data processing Dis adv: Failure of main network will stop all communication, expensive, complex.

### Network OS:

* These system run on a server
* Allows shared access
* All the users are well aware of the underlying configuration, of all other users within the network, their individual connections, etc. That's why these computers are popularly known as tightly coupled system.
* Adv: Highly stable centralized servers, security concerns are handled well, New technology and hardware upgrades are easily integrated, remote server access is possible. Dis adv: Costly, Requires regular maintenance, user has to depend on central location for most operations, latency.
* Windows server, BSD, etc.

### Real time OS:

* Used when time requirements are very string - missile, rocket, robots, etc.
* Adv:
  * Maximum consumption \(maximum utilization of devices and system thus more output from all resources\)
  * Task shifting is very less
  * Focus on application
  * Since size of programs are very small RTOS can also be used in embedded systems.
  * Error free
  * Memory allocation is best managed in these system.
* Dis adv:
  * Limited tasks
  * use heavy system resources
  * complex
  * Thread priority as these systems are very less prone to switching tasks.

## Operations

### Dual Mode

Dual mode operations allows OS to protect itself and other system components

* User mode and kernel mode
* Mode bit provided by hardware
* Some instructions designated as only executable in kernel mode to protect OS from errant users
* OS puts CPU in user mode when a user program is executing so that user program do not interfere with OS programs.

Works through API \(Application program interface\) using System call

* Provides means for user program to ask the OS to perform tasks reserved for OS on user program itself.
* Leads to invocation of trap to specific location in the interrupt vector.
* Control passes to service routine and mode bit
* After executing control set back to user mode

> Timer to prevent infinite loop resource hogging processes

### Process Management

* A process is a program in execution. It is a unit of work within the system. Program is a passive entity, process is an active entity.This means that a program can be considered as a bunch of code, or sequence of instructions, whereas a process is any such program that is currently active. A process can have several states, and is completely described in a PCB, or Process Control Block
* Process needs resources to accomplish its task \(CPU, memory, IO\)
* Process terminates requires reclaim of any reusable resources.
* Single threaded process has one program counter specifying location of next instruction. Process executes instructions sequentially one at a time.
* Provide mechanism for process synchronization, communication, deadlock handling, suspending and resuming process.

### I/O Buffering

* Process of temporarily storing data that is passing between a processor and its peripheral to smooth out the rate of speed.

### Memory Management

* Optimize CPU utilization
* Keep track of which part of memory are currently being used and by who
* Deciding which process \(or part\) and data into and out of memory.
* Allocating and deallocating memory space as needed.

### I/O Subsystem is responsible for

* I/O Scheduling: Determining a good order in which to execute a set of I/O requests - Optimal utilization.
* Buffering: It is done to cope with the speed mismatch between producer and consumer of data stream.
  * If an output device say printer is slow it has a buffer the computer can fill it and then go do other tasks while printer will read buffer.
  * If an input device say keyboard is slow it gives data to buffer the CPU reads it all at once.
  * Concurrency with two or more buffer is possible for applications to be filling one while I/O is happening on other.
* Caching: Buffer may hold only in secondary memory, caching it is faster
* Spooling \(Simultaneous Peripheral Operation Online\): It is a buffer that holds output of a device such as a printer that cannot accept continuous data. \(because I/O devices are slow to match up with speed difference\)
* Error Handling: Protected memory guards against many kind of software and hardware glitches.
* I/O protection: User cannot directly issue I/O access \(illegal\) requires high privilege.

> Micro kernel: Small in size, slow execution, easily extensible, if a service crash it does effect on working on the micro kernel, More implementation code.
>
> Monolithic: Larger, Faster, Hard to extend, If crashed It effects OS, Less implementation code.

## System Calls

* Programming interface to the service provided by OS.
* Typically written in high level language \(C++ Java\) or few low level tasks may be written in assembly.
* Mostly accessed by programs via API rather then direct system call.
* Examples:
  * unix fork\(\) or windows CreateProcess\(\) creates a separate, duplicate process \(different pid\) this new process is child process.
  * unix exit\(\) or windows ExitProcess\(\)
  * unix getpid\(\) or windows GetCurrentProcessID\(\)
  * exec\(\) system call used after a fork to replace process memory space with a new program \(replaces a process with another process, pid will be same content will differ\)

```cpp
// 1 - Fork
int main()
{
    fork();
    printf("%d ", getpid());    // Output: 5962 5973
    return 0;
}
// If there are 3 forks 2^n process would be created

// 2 - Exec
int main()
{
    printf("1. %d ", getpid());
    char *args[] = {"a", "b", "c", NULL};
    execv("path of new file ./o", args);
    printf("Back to 1");
    return 0;
}
int main()
{
    printf("2. %d", getpid());
}

// 3 - wait
int main()
{
    if (fork() == 0) exit(0);
    else
    {
        wait();
        cout << "process id has terminated";
    }
}
```

> Java Virtual Machine \(JVM\): It consists of a class loader and a java interpreter that executes the architectural neural byte code. The class loader, loads the compiled .class files from both Java API and Java Program for execution by Java interpreter.  
>   
> JVM also automatically manages garbage collection \(reclaim memory for object no longer in use\) Used on top of native OS or on chips specifically to run JAVA programs.

### OS Design and Implementation Approaches:

* Start by defining goal and specification
* Affected by choice of hardware, type of system
* Define user goal \(convenient to use, easy to learn, reliable, safe, fast\) System goal \(easy to design, implement and maintain, flexible, reliable, reliable, error free and efficient\).
* Mechanism determine how to do something, policies decide what will be done, The separation of policies from mechanism is very important principle, it allows maximum flexibility if policy decision are to be changed later.
* Once an OS is designed it has to implemented. Implementation in high level language has advantages such as easy to understand and debug.

### MS DOS:

It provides most functionality in least space, not divided into modules, its interfaces and level of functionality are not well separated in structure.

## Process

### A process includes:

* Program code \(text section\)
* Current activity represented by program counter and processor registers.
* Stack \(temporary data, function parameter, return addresses, and local variables\)
* Data section \(global variables\)
* Heap \(memory dynamically allocated during process run time\)

Program: Passive entity \(File containing list of instructions\) - executable file, becomes a process when loaded into memory.  
Process: Active entity \(Program counter + Next instruction\)

### Process State:

* new \(The process is being created\)
* running \(Instructions are being executed\)
* waiting \(Process is waiting for some event I/O\)
* ready \(Process is waiting to be assigned by processor\)
* terminated \(finished execution\)

### Process Control Block \(PCB\)

* Information associated with each process.
* Process state
* Program counter
* CPU registers
* CPU scheduling information \(Process priority, pointer to scheduling queues\)
* Memory management information \(base and limit registers, page tables\)
* Accounting information \(Amount of CPU and real time used, time limits, account numbers, process numbers\)
* I/O status information \(list of I/O devices allocated, open files, etc.\)

> PCB is kept in a memory area that is protected from the normal user access.  
> This is done because it contains important process information. Some of the OS place the PCB at the beginning of kernel stack for the process in it is a safe location.

### Scheduling Queues

* Job queue \(set of all processes\) - as it enters the system
* Ready queue \(linked list, pointer to first and last PCB's in list\)
* Device queue \(set of processes waiting for an I/O device\)

> CPU bound process: Process which requires most of time on CPU  
> I/O bound process: Process which requires most of time on I/O  
> Both I/O and CPU overhead must be maintained by scheduler.

## Schedulers

<table>
  <thead>
    <tr>
      <th style="text-align:left">Dispatcher</th>
      <th style="text-align:left">Scheduler</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <ul>
          <li>Loads the program selected by short term scheduler</li>
          <li>There&apos;s no different type of dispatcher</li>
          <li>Dependent on short term scheduler</li>
          <li>It has no specific algorithm for its implementation</li>
          <li>Time taken by dispatcher is called dispatch latency</li>
          <li>Also responsible for context switching, switching to user mode, jumping
            to proper location when process again started.</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Scheduler is something which selects a process among various processes.</li>
          <li>Types - long term, short term, medium</li>
          <li>Independent</li>
          <li>Various algos like - FCFS, SJF, RR, etc.</li>
          <li>Time taken by scheduler is usually neglected.</li>
          <li>Only work is selection of process.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Scheduler selects process from the queues to be scheduled for execution

* Long Term or Job Scheduler:
  * It brings new process to the ready state by selecting from the spool.
  * It controls degree of multi-programming i.e. no. of process present in ready queue at a point.
  * It is important that long term scheduler make a careful selection of both I/O and CPU bound process.
  * Executes at much less frequency \(in frequently\) Must be slow
* Short term or CPU scheduler:
  * It is responsible for selecting one process from ready state for scheduling it on running state. \(It only selects not load the process on running\)
  * Dispatcher is responsible for loading the process \(Switching context, switching to user mode, jumping to proper location in the newly loaded program\)
* Mid term scheduler:
  * It is responsible for suspending and resuming the process \(It mainly does swapping i.e. moving processes from main memory to disk and vice versa\)
  * Required to remove processes from memory to reduce degree of multi programming.

### Context Switches:

* CPU switches to another process \(system must save state of old process, load saved state for new process from PCB\)
* Context switch overhead
* Speed depends on - Hardware \(register\), content

### Inter process communication \(IPC\):

* Mechanism for process to communicate and to synchronize their actions
  * Shared Memory: \(Allows max speed since there's memory access\)
  * Message Passing: \(Useful for small data easier to implement, slow since it requires system calls involving kernel intervention\)
* Message passing implementation - Physical \(Shared memory, hardware bus\), Logical \(Logical properties\)
* Message passing may be either:
  * Blocking \(synchronous\): Blocking send has the sender block until the message is received, Blocking receive has the receiver block until a message is available.
  * Non Blocking \(asynchronous\): Non blocking send has the sender send the msg and continue. Non blocking receive has the receiver receive a valid msg or null.

### Direct Communication

* Links are established automatically.
* A link is associated with exactly one pair of communicating process
* Between each pair their exists exactly one link.
* The link may be unidirectional but is usually bidirectional.

### Indirect Communication

* Messages are directed and received from ports. Each port has an Id. Process can communicate only if they share that port.
* A link may be shared with many process
* Each process pair may share several lines.

### Shared Buffer

```cpp
#define BUFFER_SIZE 10
typedef struct
{
    ...
} item;
item buffer[BUFFER_SIZE];
int in = 0, out = 0;
/* (next free pos)    (first full pos)
Empty buffer when in = out
Full buffer when (in+1)%BUFFER_SIZE = out */
```

### Producer Consumer Problem:

* Producer process produces info consumed by consumer process.
* Example: Webserver producing pages and browser consuming the web pages.
* Shared buffer filled by producer and emptied by the consumer in a synchronous manner.
  * Unbounded buffer: No limit on size of the buffer \(Producer can always produce\)
  * Bounded buffer: Assumes there's a fixed buffer size \(Producer have to wait for buffer empty\)

```cpp
// Producer
item nextProduced;
while (true)
{
    while ((in+1)%BUFFER_SIZE == out);
    buffer[in] = nextProduced;
    in = (in+1)%BUFFER_SIZE;
}

// Consumer
while (true)
{
    while (in == out);
    nextConsumed = buffer[out];
    out = (out+1) % BUFFER_SIZE;
    return nextConsumed;
}
```

Problems in Producer Consumer Problem

* Producer should produce data only when buffer is not full.
* Consumer should consume only when buffer is not empty.
* Producer and consumer must not access the buffer at same time.

### Remote Procedural Calls

* RPC is a protocol that one program can use to request a service from a program located on another computer on a network without having to understand the network details.
* Follows client server architecture
* Also known as function call or subroutine call

> RMI \(Remote Method Invocation\) is a Java mechanism similar to RPCs that allow Java program on one machine to invoke a method on a remote object.

## CPU Scheduling

### Non Preemptive

* When a process completes its execution
* When a process leaves CPU voluntarily to perform some I/O operation or to wait for an event.

### Preemptive

* If process enters in the ready state either from new or waiting state i.e. high priority task.
* If a process switches from running state to ready state because time quanta expires.

### Scheduling Criteria:

* CPU utilization
* Throughput \(number of processes that complete their execution per time unit\)
* Turnaround time \(Time spent by a particular process in system\)
* Waiting time \(Time spent by process in ready queue\)
* Response time \(Amount of time when first submitted then response\) - considered major criteria

### First Come First Served \(FCFS\):

* Simplest scheduling algorithm that schedules according to arrival time.
* Process that requests the CPU first is allocated the CPU first.
* Implement using FIFO queue.
* When a process enters the ready queue, it's PCB is linked onto the tail of the queue. When the CPU is free, it is allocated to the process at the head of the queue, Running process is then removed from the queue.
* Always non-preemptive.
* Easy to understand and implement
* Suffers convoy effect: Say a process comes early but Burst time of it is very high then scheduling it first will result in convoy effect.

### Shortest Job First \(SJF\):

> SJF, LJF \(Non preemptive\)  
> SRTF, LRTF \(Preemptive\)

* Process with the shortest burst time are scheduled first.
* In case of tie FCFS is used.
* Can be used as preemptive as well as non-preemptive
* Preemptive SJF guarantees optimal ans as it is pure greedy approach. Also making it standard for other algorithms comparison.
* Cannot be implemented because burst time is not known can only be predicted by past data
* Process with larger CPU burst time requirement will go into starvation.
  * Starvation Fix: Aging -&gt; Gradually increase the priority of process that wait in system for a long time.

### Round Robin

* Always preemptive in nature
* Each process is assigned a fixed time in a cyclic way \(time quanta\)
* There's a selfish round robin \(+ priority\)
* It is designed specifically for time sharing system \(A time sharing system allows many users to share the computer resources simultaneously\)
* Ready queue is treated as a circular queue
* CPU scheduler goes around the ready queue allocating CPU to each process for a specified time quanta.
* After time quantum expires, timer interrupts the process, then the process is dispatched from process and new process comes.

### Multilevel queue

* Ready queue is partitioned \(foreground to background\) into separate queues. High priority processes are placed in top level.
* Each queue has its own scheduling algorithm - Foreground \(Round Robin\) Background \(FCFS\)
* Scheduling must be done between the queues
  * Fixed priority scheduling \(Serve all from foreground then background\)
  * Time slice
* Multi level Feedback Queue:
  * A process can move between the various queries - aging can be implemented this was
  * Multi level feedback queue schedules defined by follwoing parameters:
    * No. of queues
    * Scheduling algo for each queues
    * Method used to determine when to upgrade a process
    * Method used to determine when to demote a process
    * Method used to determine which queue a process will enter that process needs service.

## Process Synchronization

* On the basis of synchronization processes are categorized as
  * Independent Process \(Execution of one process doesn't affect the other\)
  * Cooperative Process
* Process synchronization problem arises in cooperative process also because of shared data access.
* Concurrent access to shared data may result in data inconstancy.
* Maintenance requires orderly execution of cooperative process.

### Race Condition:

* When more than one process are executing the same code or accessing same memory there is a possibility that the output of the shared variable is wrong.
* All the process using that variable doing race to say that my output is correct, this is race condition.
* In this scenario outcomes depends on the particular order in which access takes place.

### Critical Section Problem:

* Critical section is a code segment that can be accessed by only one process at a time.
* Critical section contains shared variables which must be synchronized to maintain consistency.

```text
do {
    [Entry section]
        CRITICAL SECTION
    [Exit section]
        REMAINDER SECTION
} while (true);
```

* Solution to critical section problem requires:
  * Mutual Exclusion: If process is executing in critical section then no other process is allowed then.
  * Progress: If no process in CS and other process waiting outside CS then only those process not in remainder section can participate in deciding which will enter CS next. \(P1-&gt;P2-&gt;P3-&gt;P1\)
  * Bounded waiting: Basically starvation doesnt happen, one who's waiting gets the resource.
* Test and Set is  a hardware solution to the synchronization problem. Here we share lock variable which is 0 \(unlock\) or 1 \(locked\)
* Before entering CS a process takes enquires about the lock. If locked then wait otherwise lock it execute the task and make it free.   -&gt;   fixes mutual exclusion, progress, but not bounded wait

### Turn Variable Solution:

```text
while (true)
{
    while (turn != 0);
        [CRITICAL_SECTION]
    turn = 1
        [REMAINDER_SECTION]
}

while (true)
{
    while (turn != 1);
        [CRITICAL_SECTION]
    turn = 0
        [REMAINDER_SECTION]
}

Mutual Exlusion (TRUE)
Violates Progress P0 -> P1 -> P0
```

Introducing boolean flag, if i is true means that process wants to go in CS

```text
while (true)
{
    flag[0] = true;    ......(1)
    while (flag[1]);
        [CRITICAL_SECTION]
    flag[0] = false;
        [REMAINDER_SECTION]
}

while (true)
{
    flag[1] = true;    .......(2)
    while (flag[0]);
        [CRITICAL_SECTION]
    flag[1] = false;
        [REMAINDER_SECTION]
}

Both Mutual Exclusion and Progress is maintained
One problem: Say context switch occur after (1) and then later at (2)
That will mark both the processes flag true Hence system will go in a deadlock.
```

### Peterson's Solution:

```text
do {
    flag[0] = true;    // flag 0 means p0 is interested in entering in CS
    turn = 1;
    while (turn == 1 && flag[1]);
        [CRITICAL SECTION]
    flag[0] = false;
        [REMAINDER SECTION]
} while (true);
```

### Semaphores:

* A semaphore is an integer variable that apart from initialization is accessed only through 2 atomic operations - wait\(s\) and signal\(s\)

```text
s = 1;

do{
    wait(s);
        [CRITICAL_SECTION]
    signal(s);
        [REMAINDER_SECTION]
} while (true);

wait(s):
    while(s <= 0);
    s = s-1;

signal(s):
    s = s+1;
```

* Disadvantages:
  * Requires busy waiting
  * Bounded wait violated
* Applications:

  * Solving critical section problem \(above\)
  * Deciding order of execution: Say we have 3 processes and we want P2 -&gt; P1 -&gt; P3 OS considers all of them equal chance we want to maintain order

  ```text
   wait(s1)                       wait(s2)
     p1                p2            p3
  signal(s2)       signal(s1)

  s1 = 0, s2 = 0
  ```

  * For managing resource: If we want n resources to have access just initialize s with n

### Reader Writer Problem

* Concurrent read is fine but no concurrent write i.e. no RW WR WW
* This problem is CS for writers but not for readers multiple readers can be there at once.

```text
write = 1

For writer:
wait(write)
    [WRITE]
signal(write)

For reader:
wait(mutex)    // mutex is used to synchronize between different reader
readCnt++;
if (readCnt == 1)
    wait(write)    // If we're first reader then don't allow other writer
signal(mutex)
    [READ]
wait(mutex)
readCnt--;
if (readCnt == 0) signal(write)
signal(mutex)

For reader above few lines act as a small CS to update readerCnt which is shared
```

### Dining Philosopher's Problem

* 5 philosophers are sitting at dining circular table. In center there's rice. Each philosopher has 1 chopstick so when hungry pick closest pair and eat.
* Represent each chopstick with a semaphore. A philosopher tries to grab a chopstick by executing a wait\(\) operation on that semaphore. she releases the chopstick by signal\(\) operation on that semaphore.
* Shared data -&gt; Bowl of rice \(data set\) and semaphore chopstick\[5\] initialized to 1

```text
do {
    wait(chopstick[i]);
    wait(chopstick[(i+1)%5]);
        [EAT]
    signal(chopstick[i]);
    signal(chopstick[(i+1)%5]);
        [THINK]
} while (true);
```

* It fails and can cause deadlock, suppose all 5 philosopher became hungry simultaneously and grabs chopstick. When each philosopher tries to grab her right chopstick she will be delayed forever.
* Solution to above problem:
  * Allow at most 4 philosopher sitting simultaneously
  * Allow philosopher to pick up her chopsticks only if both sticks are available.
  * Use asymmetric solution i.e. odd philosopher picks her left and then right whereas even one picks in reverse order

<table>
  <thead>
    <tr>
      <th style="text-align:left">Mutex</th>
      <th style="text-align:left">Semaphore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <ul>
          <li>A program object that allow multiple process to take turns to share the
            same resources</li>
          <li>Locking mechanism</li>
          <li>An object</li>
          <li>No categorization</li>
          <li>If the mutex is locked the process that requests the lock waits until
            the system releases the lock</li>
          <li>Process uses acquire() and release() to access and release mutex.</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li>A variable that is used to control access to a common resource by multiple
            processes in a concurrent system such as multi tasking OS.</li>
          <li>Signalling mechanism</li>
          <li>An integer</li>
          <li>Categorized as binary and counting semaphore</li>
          <li>If the semaphore value is 0 the process perform wait() operation until
            the semaphore becomes &gt; 0</li>
          <li>Process use wait() and signal() to modify semaphore.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

### Monitors

* It is a collection of condition variables and procedures combined together in a special kind of module or a package.
* The process running outside the monitor can't access the internal variable of the monitor but can call procedures of the monitor.
* Only one process at a time can execute code inside monitors

Two different operations are performed on condition variables of monitor

* Wait: Process performing wait operation on any condition variable are suspended. Suspended process are placed in block queue. \(each condition variable has its own block queue\)
* Signal: When a process performs signal operation on condition variable one of blocked process is given chance

Adv: Makes parallel programming easier and part less error prone than semaphore.  
Disadv: Have to be implemented as part of programming language. Compiler must generate code for them adding extra burden.

### Monitor Solution to Dining Philosopher

```cpp
monitor DP
{
    enum {THINKING, HUNGARY, EATING} state[5];
    condition self[5]
    void pickup(int i)
    {
        state[i] = HUNGARY;
        test(i);
        if (state[i] != EATING) self[i].wait;
    }
    void putdown(int i)
    {
        state[i] = THINKING;
        test[(i+4)%5];
        test[(i+1)%5];
    }
    void test(int i)
    {
        if (state[(i+4)%5] != EATING && state[i] == HUNGARY && state[(i+1)%5] != EATING)
            self[i].signal();
    }
    void initializeCode()
    {
        for (int i = 0; i < 5; ++i) state[i] = THINKING;
    }
};
```

## Deadlock

* In a multi programming system a number of process compete for limited no. resources and if a resource is not available at that instance then the process enters into waiting state.
* If a process is unable to change its state indefinitely because the resource requested by it are held by another waiting process then system is said to be in deadlock.

### Necessary condition for deadlock

* Mutual Exclusion: At least one resource type in the system which can be used in non-shareable i.e. mutual exclusion \(one at a time / one by one\) eg: printer
* Hold and Wait: A process is currently holding at least one resource and requesting additional resources which are held by other process otherwise starvation will occur.
* No preemptive: A resource cannot be preempted from a process by any other process. Resources can be released only voluntarily by the process holding it.
* Circular wait: Each process must be waiting for a resource which is being held by another process, which in turn is waiting for the first process to release resources.

### Deadlock Handling Methods:

Based on severity and frequency

* Prevention: Means design such system which violate at least one of four condition of deadlock and ensure independence from deadlock -&gt; RTOS
* Avoidance: System maintains a set of data using which it takes a decision whether to entertain a new request or not be in safe state.
* Detection and Recovery: Here we wait until a deadlock occur and after detecting we do something.
* Ignorance: Most OS unix and windows. Application developers need to handle deadlock themselves.

### Violating Deadlock Conditions:

* Mutual Exclusion: Make it shareable
* Hold and Wait:
  * Conservative approach: Process is allowed to start executing iff it has acquired all resources \(less efficient, not implementable because we cannot determine what resources are needed beforehand, easy to understand\)
  * Do not hold: Process will acquire only desired resources but before making any fresh request it must release all the resources that it is currently holding. \(Efficient and implementable\)
  * Wait timeout: we place a max time upto which a process can wait after which process must release all the holding resources and exit.
* No Preemption:
  * Forceful preemption: We allow a process to forcefully preempt the resources holding by other process.
  * This method may be used by higher priority process.
  * The process which are in waiting state must be selected as a victom instead of process in running stack.
* Circular Wait:
  * Give natural number label to every resource R1 R2 R3... and allow every process to either take only in increasing order or decreasing order. Say P1 & P2 both wants R1 & R2. first P1 tries to fetch R1 it will get if P2 tries now he cant get R1 so he will release holded ones and exit.

