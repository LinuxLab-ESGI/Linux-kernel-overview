# Linux-kernel-overview

A general overview of a kernel and why the Linux kernel stands out.

# The kernel overview

## Introduction

The kernel is the central layer of an operating system (OS) - controlling the resources of the machine such as the CPU, the memory and the devices - this central component of a computer system is responsible for 'running' or 'executing' programs. The kernel takes responsibility for deciding at any time which of the many running programs should be allocated to the processor or processors.

The shell interface resides between the users and the kernel, it has a layer of utilities or useful programs such as editors, compilers for programming languages and programs that you write yourself.

## Types of kernels

The types of kernels are defined according to the code being execucted in the hardware's priviledged mode, also defined as the total volume of the kernel code.

### a) Microkernel  

A microkernel provides minimal services, such as defining memory address spaces, interprocess communication (IPC) and process management. All other functions, such as hardware management, are implemented as processes running independently of the kernel.

### b) Monolithic  

Monolithic kernels contain all the OS core functions and the device drivers (small programs that allow the OS to interact with hardware devices, such as disk drives, video cards and printers). In a monolithic kernel, all OS services run along with the main kernel thread, thus also residing in the same memory area. This approach provides rich and powerful hardware access.

The main disadvantages of monolithic kernels are the dependencies between system components – a bug in a device driver might crash the entire system – and the fact that large kernels can become very difficult to maintain.

Monolithic kernels have traditionally been used by Unix-like OS such as Linux, OS X, SunOS - now known as Oracle Solaris- or the no longer developed Berkeley Software Distribution (BSD).

### c) Hybrid  

Hybrid kernels are similar to microkernels, except that they include additional code in the kernel space for optimizational purposes of the code run time. These kernels represent a compromise that was implemented by some developers before it was demonstrated that pure microkernels can provide high performance.

Hybrid kernels should not be confused with monolithic kernels that can load modules after booting (such as Linux).

### d) Exokernel  

Exokernels remain an experimental approach to OS design. They differ from the other types of kernels in that their functionality is limited to the protection and multiplexing of the raw hardware and they provide no hardware abstractions on top of which applications can be constructed. This separation of hardware protection from hardware management enables application developers to determine how to make the most efficient use of the available hardware for each specific program.

Exokernels in themselves are extremely small. However, they are accompanied by library OS, which provide application developers with the conventional functionalities of a complete OS. A major advantage of exokernel-based systems is that they can incorporate multiple library OS, each exporting a different API (application programming interface), such as one for Linux and one for Microsoft Windows, thus making it possible to simultaneously run both Linux and Windows applications.

### e) Nanokernel

Nanokernels are a small capability-based system originally designed to provide security sufficient to support mutually antagonistic users. They can run in less than 100 Kilobytes of memory, including all of the system privileged codes, plus additional facilities necessary to support OS and applications. Its original implementation was motivated by the need to provide security, reliability, and 24-hour availability for applications.

It is characterized by a small set of powerful and highly optimized primitives that allow it to achieve performance competitive with the macrokernel OS that it replaces. On restart, all processes are restored to their exact state at the time of checkpoint, including registers and virtual memory.

> The term nanokernel first appeared in the paper "The KeyKOS NanoKernel Architecture." The KeyKOS nanokernel is a capability-based, object-oriented OS that has been on the market since 1983. It was implemented to address requirements, such as reliability, security and consistent availability, for applications on the Tymnet hosts. It is roughly 20,000 lines of C code, which includes checkpoint, capability and virtual memory support.

## Composition of a kernel

### a) Components

Probably the most important parts of the kernel (nothing else works without them) are memory management and process management. Memory management takes care of assigning memory areas and swap space areas to processes, parts of the kernel, and for the buffer cache. Process management creates processes, and implements multitasking by switching the active process on the processor

### b) Main function

The main function of the kernel are **memory management** and **process management**, as nothing else works without them.

Memory management takes care of assigning memory areas and swap space areas to processes, parts of the kernel, and for the buffer cache such as RAM memory management to ensure the correct functionality of programs and running processes.

>The kernel virtual memory is the part of the OS that is always resident in memory. The top 1/4 of the address space is reserved for the kernel. Application programs are not allowed to read or write the contents of this area or to directly call functions defined in the kernel code.

Process management creates processes, and implements multitasking by switching the active process on the processor. This is helpful in two ways:

* Firstly, it helps us quantify the overall busyness of the system therefore letting us know if we have insufficient processing power and allowing us to adjust our needs (upgrading our CPU or reducing the user experience).

* Secondly, it helps quantifying how the processor is shared between computer programs. High CPU usage by a single program may indicate that it is highly demanding of processing power or that it may malfunction such is the case of infinite loops. CPU time allows measurement of the processing power a single program requires, eliminating interference, such as time executed waiting for input or being suspended to allow other programs to run.

### c) Communication with the OS

 At the OS level, the kernel transfers control from one user process to another via context switches, this is the process management.

 At the lowest level, the kernel contains a hardware device driver for each kind of hardware it supports. Since the world is full of different kinds of hardware, the number of hardware device drivers is large. There are often many otherwise similar pieces of hardware that differ in how they are controlled by software. The similarities make it possible to have general classes of drivers that support similar operations; each member of the class has the same interface to the rest of the kernel but differs in what it needs to do to implement them.

 For example, all disk drivers look alike to the rest of the kernel, i.e., they all have operations like `initialize the drive`, `read sector N`, and `write sector N`. Some software services provided by the kernel itself have similar properties, and can therefore be abstracted into classes. For example, the various network protocols have been abstracted into one programming interface, the BSD socket library.

# The linux kernel: a closer look

## Introduction

As we have already established, the kernel is the software that starts up when you boot your computer and manages the programs you use so they can communicate effectively and simply with your computer hardware. Other components, such as administrative commands and applications, can be added  to the kernel from free and open source software projects. The GNU project, in particular, contributed many components that are now in Linux.

> The GNU project added such things as graphical user interfaces (GUIs), administrative utilities (adding users, managing disks, install software, securing and managing your computer, ...), applications (productivity tools, web browsers, chat windows, etc), server features and programming tools to the Linux OS.

## The history behind the Linux kernel

The Linux OS is a high performance, free and open-source Unix-like OS that started as a hobby by Linus Torvalds in 1991. He started to work on a UNIX-like kernel because he wanted to be able to use the same kind of operating system on his home PC that he used at school. At the time, Linus was using Minix, but he wanted to go beyond what the Minix standards permitted.

The Linux kernel was the last — and the most important — piece of code that was needed to complete a whole UNIX-like OS under the GNU Public License. When people started putting together distributions, the name Linux is what stuck  and not GNU. However,  some distributions such as Debian refer to themselves as GNU/Linux distributions.

As it is written in C, it can be ported to new systems without extensive code modification, this created a wider audience for Linux as an OS.

>Slackware Linux, which was first released in April, 1993, is one of the oldest surviving Linux distributions.

## The modules

In the words of Brian Kernighan the Linux kernel can be viewed as a kind of building block with what you can create things - this can be done so in unobvious ways which makes the system overall very flexible.  

### a) Definition

The Linux kernel is a monolithic kernel with a modular design (e.g, it can insert and remove loadable kernel modules at runtime), supporting most features once only available in closed source kernels of non-free operating systems maintaining a manageable size by including only a small set of drivers active.

Modules or loadable kernel modules (LKM) can be loaded and unloaded at runtime by using the `menuconfig` command during startup : they extend the functionality of the kernel without the need to reboot the system. This helps the optimization of available memory by only keeping active the necessary modules and further expands the customization of the Linux kernel to load or unload modules

Without modules, we would have to build monolithic kernels and add new functionality directly into the kernel image. Besides having larger kernels, this has the disadvantage of requiring us to rebuild and reboot the kernel every time we want new functionality.

Typically, the Linux kernel loads kernel modules from `/lib/modules/$kernel-version/`.

LKMs are .ko file extension and can be found in the `/lib/modules` directory.

Linux supports thousands of hardware devices, yet keeps the kernel a manageable size by including only a small set of drivers in the active kernel. Using LMK, the kernel can add support for other hardware as needed. Modules can be loaded and unloaded on demand, as hardware is added and removed.

### b) Stating modules

A program usually begins with a `main()` function, executes a bunch of instructions and terminates upon completion of those instructions. Kernel modules work a bit differently. A module always begin with either the `init_module` or the function you specify with `module_init` call. This is the entry function for modules; it tells the kernel what functionality the module provides and sets up the kernel to run the module's functions when they're needed. Once it does this, entry function returns and the module does nothing until the kernel wants to do something with the code that the module provides.

All modules end by calling either `cleanup_module` or the function you specify with the `module_exit` call. This is the exit function for modules; it undoes whatever entry function did. It unregisters the functionality that the entry function registered.

__________
Updated : 06/02/2021 Author: andrea-bsierra
