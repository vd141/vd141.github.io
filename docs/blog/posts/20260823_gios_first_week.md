---
title: GIOS Syllabus Week
date:
  created: 2026-08-23
tags:
  - Notes
  - Operating Systems
---
The first week of GIOS is upon us! Going forward, I will be using these blog posts to document my knowledge as I progress through the class.

I take academic integrity seriously, so I will not be posting any code snippets from assignments on my blog.

Working through the modules:

  - 0.5 - 2 hours of lectures per week

  - 3 projects each with a 3 week timeline

## GIOS Diagnostic Quiz
C concepts to know:

- Structs, arrays, pointers, and reference types

    - a **struct** is a data structure that contains member variables

    - **arrays** are iterable data structures that store data in contiguous memory

    - **pointers** are references to variables. they are memory addresses, and they are declared by specifying the type of variable they point to followed by a *. To get the address of a variable, prepend the variable with an ampersand '&'. Use pointers to pass variables by reference. Dereferencing a null pointer is undefined behavior in C, so here are several ways to address it

        - initialize the pointer immediately

        - perform safe null checks (verify that pointer is not null before extracting its value)

        - set pointers to null after freeing memory. when a pointer's memory is cleared, it becomes a dangling pointer. anything it points to is no longer governed by stack policy.

  - **reference types** Google search claims that there are no "reference types" in C. So this seems like what I discussed in the description about pointers, namely how a pointer variable is declared with the object type of the object it is pointing to

  - File I/O
      - Functions are called from the <stdio.h> library
      - file workflows typically follow this sequence: open, verify, access, close
      - use the return values of functions like fgetc or fgets to control loops instead of relying solely on feof(fp).
      - feof() is only set to true only after a read operation attempts to fetch data and fails
      ``` C
      // opening a file returns a file pointer which can be accessed
      // the second argument is a string that specifies the access mode.
        // "w": write - creates a new file or overwrites an existing file
        // "a": append - writes to the end of the file or creates a new file if missing
        // "r": read - reads the file, returns NULL if the file does not exist
        // "r+": read and write - reads and writes to the file, returns NULL if the file does not exist
        // "w+": write and read - creates a new file or overwrites an existing file
        // "a+": append and read - writes to the end of the file or creates a new file if missing
      FILE *fp = fopen("a_file.txt", "w");

      // verify that the file open was successful (did not return a NULL pointer)
      if (fp == NULL) {
        printf("Error opening file!\n");
        return 1;
      }

      // writing functions
      // fputc(int char, FILE *fp) - writes a character to the file. can pass in a character literal/variable or an integer for the ASCII value
      // fputs(const char *str, FILE *fp) - writes a string to the file
      // fprintf(FILE *fp, const char *format, ...) - writes formatted data to the file. first argument is output stream (can be file or console), second argument is a format string, and the rest are the values to be formatted. can use format specifiers like %d for integers, %f for floats, %s for strings, etc.
      // fwrite(const void *ptr, size_t size, size_t count, FILE *fp) - writes data from memory to file. first arg is pointer to block of memory, second is size of each element in bytes, count is the total number of elements to write to file, stream is the pointer to the file where the data will be written
      // * size_t is a specialized unsigned integer type that can represent the size of any valid object or memory block on the host system. it is the official type returned by the sizeof operator

      // reading functions
      // fgetc(FILE *fp) - reads a character from the file
      // fgets(char *str, int n, FILE *fp) - reads a string from the file. first arg is pointer to destination string, n is the max number of characters to read including the null terminator, fp is the pointer to the source
      // fscanf(FILE *fp, const char *format, ...) - reads formatted data. example: int items_read = fscanf(file, "%49s %d %f", name, &age, &gpa);
      // fread(void *ptr, size_t size, size_t count, FILE *fp) - reads data from file into memory. first arg is pointer to block of memory, second is size of each element in bytes, count is the total number of elements to read from file, stream is the pointer to the file from which the data will be read

      // file navigation
      // fseek(FILE *fp, long offset, int whence) - moves the file pointer to a specific location in the file. whence can be SEEK_SET (beginning of file), SEEK_CUR (current position), or SEEK_END (end of file)
      // ftell(FILE *fp) - returns the current position of the file pointer
      // rewind(FILE *fp) - sets the file pointer to the beginning of the file

      // close the file when finished to avoid dangling pointers
      fclose(fp);
      return 0;
      ```
  - Use of command line parameters
      - argc tracks the total number of arguments passed, including the program name itself
      - argv is an array of null-terminated strings representing the arguments passed to the program
      - argv[argc - 1] gives the last argument passed to the program
      - argv[argc] is guaranteed to be a null pointer
  ``` C
    int main(int argc, char *argv[]) {
      // Your code here
      return 0;
    }
  ```
  - Pass-by-reference and pass-by-value
      - all arguments in c are passed by value, but arguments can be passed by reference by explicitly passing pointers
      - passing by value copies entire objects into the function (high overhead)
  - Dynamic memory allocation using malloc()
      - dynamically allocate memory for objects at runtime
      - free allocated memory using free() to avoid memory leaks (write one free for every malloc)
      - use sizeof()
  - Use of C libraries
      - `#include<stdio.h>`
  - Debugging programs
      - compile with flags
          - `# gcc -g -Wall -Wextra program.c -o program`
      - run the program with gdb. gdb is an interactive debugger that is controlled via gdb commands in the gdb command line interface. use a cheat sheet for commands
          - `# gdb ./program`
  - Reading documentation
  - Iterative design
      - design and test small bits of code as you build up to the full functionality. can save lots of time when the program becomes more complex. also helps you understand the smaller details of the program.
  - Good coding standards
      - use void in function parameters if function takes no arguments
      - replace hard-coded numbers with const variables when possible
      - check return values
      - manage memory carefully

## C Programming Examples
### Linked List
  - it's a dynamic data structure whose length can be modified at runtime
  - linked lists are preferred when the volume of data to be stored cannot be determined in advance
  - a node contains the data and the pointer to the next node
  - this is how you can create a node `struct test_struct *ptr = (struct test_struct*)malloc(sizeof(struct test_struct));`
    - malloc returns a void pointer, which is then typecast into a struct test_struct pointer. 
  - now the node can be updated like so:
    - ``` C
      ptr->val = val;
      ptr->next = NULL;
      ```
  - The program can traverse the list by updating the current pointer to the current node's next pointer, then checking the value of that node. If there is no match, the pointer can continue to be updated. If there is a match, the loop can be broken. The stop condition of the loop is if the current pointer is null.
  - Because nodes are created using malloc, a node can be deleted using free(). The adjacent nodes should be linked first. 
  - the first node is always made accessible through the use of a global head pointer.
  - ```
    #include<stdio.h>
    #include<stdlib.h>
    #include<stdbool.h>

    struct test_struct
    {
        int val;
        struct test_struct *next;
    };

    struct test_struct *head = NULL;
    struct test_struct *curr = NULL;

    struct test_struct* create_list(int val)
    {
        printf("\n creating list with headnode as [%d]\n",val);
        struct test_struct *ptr = (struct test_struct*)malloc(sizeof(struct test_struct));
        if(NULL == ptr) // null checking is performed immediately after malloc
        {
            printf("\n Node creation failed \n");
            return NULL;
        }
        ptr->val = val;
        ptr->next = NULL;

        head = curr = ptr;
        return ptr;
    }

    struct test_struct* add_to_list(int val, bool add_to_end)
    {
        if(NULL == head)
        {
            return (create_list(val));
        }

        if(add_to_end)
            printf("\n Adding node to end of list with value [%d]\n",val);
        else
            printf("\n Adding node to beginning of list with value [%d]\n",val);

        struct test_struct *ptr = (struct test_struct*)malloc(sizeof(struct test_struct));
        if(NULL == ptr)
        {
            printf("\n Node creation failed \n");
            return NULL;
        }
        ptr->val = val;
        ptr->next = NULL;

        if(add_to_end)
        {
            curr->next = ptr;
            curr = ptr;
        }
        else
        {
            ptr->next = head;
            head = ptr;
        }
        return ptr;
    }

    struct test_struct* search_in_list(int val, struct test_struct **prev)
    {
        struct test_struct *ptr = head;
        struct test_struct *tmp = NULL;
        bool found = false;

        printf("\n Searching the list for value [%d] \n",val);

        while(ptr != NULL)
        {
            if(ptr->val == val)
            {
                found = true;
                break;
            }
            else
            {
                tmp = ptr;
                ptr = ptr->next;
            }
        }

        if(true == found)
        {
            if(prev)
                *prev = tmp;
            return ptr;
        }
        else
        {
            return NULL;
        }
    }

    int delete_from_list(int val)
    {
        struct test_struct *prev = NULL;
        struct test_struct *del = NULL;

        printf("\n Deleting value [%d] from list\n",val);

        del = search_in_list(val,&prev);
        if(del == NULL)
        {
            return -1;
        }
        else
        {
            if(prev != NULL)
                prev->next = del->next;

            if(del == curr)
            {
                curr = prev;
            }
            else if(del == head)
            {
                head = del->next;
            }
        }

        free(del);
        del = NULL;

        return 0;
    }

    void print_list(void)
    {
        struct test_struct *ptr = head;

        printf("\n -------Printing list Start------- \n");
        while(ptr != NULL)
        {
            printf("\n [%d] \n",ptr->val);
            ptr = ptr->next;
        }
        printf("\n -------Printing list End------- \n");

        return;
    }

    int main(void)
    {
        int i = 0, ret = 0;
        struct test_struct *ptr = NULL;

        print_list();

        for(i = 5; i<10; i++)
            add_to_list(i,true);

        print_list();

        for(i = 4; i>0; i--)
            add_to_list(i,false);

        print_list();

        for(i = 1; i<10; i += 4)
        {
            ptr = search_in_list(i, NULL);
            if(NULL == ptr)
            {
                printf("\n Search [val = %d] failed, no such element found\n",i);
            }
            else
            {
                printf("\n Search passed [val = %d]\n",ptr->val);
            }

            print_list();

            ret = delete_from_list(i);
            if(ret != 0)
            {
                printf("\n delete [val = %d] failed, no such element found\n",i);
            }
            else
            {
                printf("\n delete [val = %d]  passed \n",i);
            }

            print_list();
        }

        return 0;
    }
    ```
### Working with Time and Dates
  - referenced from [Beej](https://beej.us/guide/bgc/html/split-wide/date-and-time-functionality.html)
  - i like this resource better [codingunit c time tutorial](https://web.archive.org/web/20250326205729/https://www.codingunit.com/c-tutorial-how-to-use-time-and-date-in-c)
  - use the `#include<time.h>` header
  - GMT/UTC is a universally agreed-upon time
    - used for events that happen once
  - local time is used for events that happen the same time in every time zone (i.e. noon)
  - there are two main types when it comes to time:
    - time_t, which represents the number of seconds since Epoch (Jan 1, 1970 UTC). it can also be negative to denote times before epoch.
    - struct tm, which holds the components of calendar time
  - some common time operations in c:
    ``` C
    time_t now;  // Variable to hold the time now

    now = time(NULL);  // You can get it like this...

    time(&now);        // ...or this. Same as the previous line.

    printf("Current time: %s", ctime(&now)); // ctime converts time_t into a human-readable string

    // convert a struct tm into time_t using the mktime command
    struct tm str_time;
    time_t time_of_day;

    str_time.tm_year = 2012-1900;
    str_time.tm_mon = 6;
    str_time.tm_mday = 5;
    str_time.tm_hour = 10;
    str_time.tm_min = 3;
    str_time.tm_sec = 5;
    str_time.tm_isdst = 0;

    time_of_day = mktime(&str_time);
    printf(ctime(&time_of_day));

    // note that if the tm_hour and the output of ctime differ by one hour, daylight savings time is in effect. this could depend on the date of the year and/or the tm_isdst flag.

    // using difftime to measure runtime of code block
    time_t start,end;
    volatile long unsigned counter;

    start = time(NULL);

    for(counter = 0; counter < 500000000; counter++)
        ; /* Do nothing, just loop */

    end = time(NULL);
    printf("The loop used %f seconds.\n", difftime(end, start));
    return 0;

    // converting time_t to utc and using timezones
    time_t raw_time;
    struct tm *ptr_ts;

    time ( &raw_time );
    ptr_ts = gmtime ( &raw_time ); // converts to greenwich mean time

    printf ("Time Los Angeles: %2d:%02d\n",
            ptr_ts->tm_hour+PST, ptr_ts->tm_min);
    printf ("Time Amsterdam: %2d:%02d\n",
            ptr_ts->tm_hour+CET, ptr_ts->tm_min);
    return 0;
    ```
  - conversions between the two types can be done (see guide for details)
### Psuedo-Random Numbers
  - rand() returns a random number between 0 and RAND_MAX (guaranteed to be at least 32767) - depends on implementation of c library
  ``` C
  int rand(void) // accepts no arguments
  ```
### String Functions
- [Source](https://en.wikibooks.org/wiki/A_Little_C_Primer/C_String_Function_Library)
```C
   strlen()    Get length of a string.
   strcpy()    Copy one string to another.
   strcat()    Link together (concatenate) two strings.
   strcmp()    Compare two strings.
   strchr()    Find character in string.
   strstr()    Find string in string.
   strlwr()    Convert string to lowercase.
   strupr()    Convert string to uppercase.
```

## C++ Programming
### C++ strings
- [Reference](https://en.wikibooks.org/wiki/C%2B%2B_Programming/Code/IO/Streams/string)
### C++ OOP
- [Reference](https://www.w3schools.com/cpp/cpp_oop.asp)
### C++ class
- [Reference](https://en.wikibooks.org/wiki/C%2B%2B_Programming/Classes)
### Pointers in C++
- [Reference](https://www.tutorialspoint.com/article/pointers-smart-pointers-and-shared-pointers-in-cplusplus)
    - C++ pointers are just like C pointers!
    - Smart pointer is a pointer that is wrapped in a class that takes care of
        - lifetime of dynamically allocated memory
### Lock guard
- [Reference](https://en.cppreference.com/cpp/thread/lock_guard)
    - a scoped mutex wrapper that provides a safe, exception proof way to lock and unlock a mutex using Resource Acquisition is Initialization (RAII)
    - a mutex is a synchronization primitive used in multi-threaded programming to prevent concurrent access to shared resources

## Command Line Concepts
- Read man pages (man)
- Navigate directories (cd, ls, pwd)
- Move and copy files (mv, cp)
- Adjust permissions or groups (chown, chmod)
- Run executables (gcc, C executables, or other tools)

## Makefile concepts
- [Colby Make tutorial (for usage examples)](https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/)
- [The Make File tutorial (intuitive explanations)](https://makefiletutorial.com/)
- projects must be submitted with makefiles to automate the build process of their c programs
- the goal of makefiles is to compile whatever files need to be compiled, based on what files have changed
    - i've always used a single command on the CLI to compile my code, so i am also trying to figure out what makefiles do better
- the c makefile uses a combination of rules and macros (variables) to compile. rules specify targets, dependencies, and the commands needed to create them. commands should alwayas be indented with tabs.
- things to know:
    - Targets and dependencies
        - the list of dependencies following a target declaration tells make to check if those files have changed since the last compilation. if they have, then make will recompile the target. if those files have not changed, make will not recompile the target (if the target file already exists), even if the target is explicitly called out in the make command
    - Comments (always useful!)
        - each comment line starts with `#`
    - Variables (compiler, flags, etc.) - AKA macros are defined at the top of the makefile
    - Calling make from the command line
        - calling `make` with no arguments executes the first rule in the file
    ``` bash
    # specify the compiler
    CC=gcc

    # specify options for the compiler
    CFLAGS=-c -Wall

    all: hello

    hello: main.o hello.o
        $(CC) main.o hello.o -o hello

    main.o: main.cpp
        $(CC) $(CFLAGS) main.cpp

    hello.o: hello.cpp
        $(CC) $(CFLAGS) hello.cpp

    clean:
        rm -rf *o hello
    ```

## Learning goals!
The course will answer three questions:
    - What are operating systems?
    - Why are they needed?
    - How are they designed and implemented to provide the required functionality?

To provide its intended functionality, an operating system uses abstractions, mechanisms, and policies for
    - processes and process management
    - threads and concurrency, and in general challenges related to multithreading
    - managing hardware (CPU, memory) resources via scheduling and memory management
    - OS services such as communication and I/O
    - OS support for distributed services including remote procedure calls (RPC), distributed filesystems, and distributed memory
    - introduce systems software that is used in data center and cloud environments

Practical projects are intended to supplement the theory and impart an appreciation for operating systems. projects will provide challenges related to 
    - threads, concurrency, and synchronization
    - multiprocesses on a single node (like a server machine): inter-process synchronization, scheduling...
    - multi-node mechanisms: remote procedure calls, ...
    - measuring and evaluating design decisions with respect to performance

To carry out these ends, the programming projects are to be written in C on a Linux operating system. standard linux libraries will be used, such as pthreads.

There is no required textbook, but Ada recommends the "dinosaur" books: 
- Operating Systems Concepts, 
- Operating Systems Concepts Essentials. 
She also recommends the Tannenbaum Modern Operating Systems textbook and the [Operating Systems: Three Easy Pieces](https://pages.cs.wisc.edu/~remzi/OSTEP/)

Seminal research papers will be provided, as well as tutorials and technology surveys. 

Online resources will be provided for C programming, PThreads, and other libraries.

A toy shop metaphor will be used when describing the operating system. 

REMIMDER: READ PIAZZA POSTS

## P1L2 Playlist: OS Components, Abstractions, Arbitrations, and System Calls
Dipping the toe in the water!

What is an operating system and what role does it play in computers?

- it's a piece of software that abstracts (simplifies conceptually) and arbitrates (manages) the underlying hardware
- it offers many abstractions and arbitrations for the various underlying hardware systems
- how does it manage and arbitrate the underlying hardware?
    - it directs operational resources, which include CPU, memory, and peripheral devices
    - it enforces working policies, such as fair resource access, limits to resource usage with respect to processes
    - manages the difficult of complex tasks by abstracting hardware details. system calls are such an abstraction
- what are the underlying hardware pieces of a computer system? all of these will be used by multiple applications, except in some specific environments like embedded platforms or sensors
    - processor (1 or more). today's CPUs have multiple cores (processing element)
    - main memory
    - network interconnects, such as ethernet port or wifi card
    - graphics processing cards (GPU)
    - storage devices such as HDD, SSD, flash USB drives
- what kind of applications use these resources?
    - on a client computer, this could be a browser, text editor, video conferencing apps
    - on a data center server, this could be a database server, web server, a file storage system, a computationally intensive simulation, etc.
    - an os is the software that sits between the hardware and the application software
- what kind of hardware complexity does the operating system hide?
    - an example would be disk sectors or blocks when saving output of a computation in you program
        - it manages a higher level abstraction called a file, which has read and write functions
    - another example would be the bits and packets that are sent after receiving a request from a client
        - Os abstracts this into a send/receive socket
- how does the OS manage the underlying hardware?
    - decides which and how many of the hardware resources to use
    - for example, it decides how much memory to use for an application, and schedules application tasks on the CPU
    - it also decides when the various applications get to access the hardware
    - also provides isolation and protection, meaning that the various applications that are using the same hardware resources don't interfere with each other. for example, the os would allocate certain addresses in memory to specific applications
    - managing hardware is also important on devices that were once considered embedded devices, such as our phones. 
- the os hides hardware details from the application, and has policies that govern how applications may interact with the underlying hardware
- is it part of the operating system or not?
    - part of os:
        - device driver
        - file system
        - scheduler
    - not part of os:
        - file editor (application)
        - cache memory (hardware)
        - web browser (application)
- is it an abstraction or arbitration?
    - arbitration
        - distributing memory between multiple processes
    - abstraction
        - supporting different types of speakers (applications don't need to worry about speaker details)
        - interchangeable access of hard disk or ssd (application doesn't need to worry about storage details)

What are some examples of operating systems?

- operating systems differ by the kind of environment they target. for example, some os's are for pc desktops, some are for servers, some are for embedded devices
- some common desktop operating systems are microsoft windows, unix-based (mac osx, linux)
- Ada considers embedded operating systems to include: ios, android, and symbian

What are the key components of the operating system?

- an operating system supports a number of higher level abstractions and mechanisms that operate on top of these abstractions
- abstractions
    - correspond to applications
        - process
        - thread
    - corresponds to the hardware
        - file
        - socket
        - memory page
- mechanisms
    - for applications
        - create (or launch an application)
        - schedule (to run it on the CPU)
    - for hardware
        - open
        - write
        - allocate
- policies govern how mechanisms will be used to manage the underlying hardware
    - a policy controls the max number of sockets that a process can have access to
    - control which data can be removed from memory based on some algorithm like least-recently used (LRU), earliest deadline first (EDF)

Memory management example:

- abstraction: 
    - memory page - corresponds to some addressable region of memory with fixed size
- mechanisms to operate on that page: 
    - allocates that page in dram, maps that page into the address space of the process (allows process to access the physical memory that corresponds to the contents of that page)
    - page can be moved to different locations in physical memory
    - page can be stored on disk if we need to make room for other content in physical memory
- policies: 
    - it's faster to access items in memory, so there must be some policy governing when items get moved from memory to disk
    - least recently used -LRU is such a policy
        - rationale is that pages that have not been used in a while are likely to not be used imminently
    - the act of moving items between disk and memory is known as "swapping"

What are some design and implementation considerations of operating systems?

- separation of mechanism and policy
    - this makes mechanisms flexible, meaning they can be used in different types of policies
    - memory management policies: LRU, LFU (least frequently used), random
        - for these policies, we might want to have a mechanism that tracks the frequency or the time of memory access
    - knowing this, policies must be defined for a "common case". some considerations:
        - where will the os be used?
        - what machine will it be run on? what resources are available on that machine?
        - what will the user want to execute on that machine?
        - what are the workload requirements?
    - with answers to these questions, and an awareness of what mechanisms are available, a policy can be defined

How does the OS manage access to the hardware?

- it uses a user/kernel protection boundary
- "unprivileged mode" is designated for user-level applications. this is one side of the boundary. aka user mode
- "privileged mode" is reserved for kernel-level operating system actions. aka kernel mode
- traps, system calls, and signals
- user-kernel switch is supported by hardware, meaning there is a bit in the CPU that reflects whether an operation has access to the hardware.
    - if an operation only has user-level privileges but tries to perform a privileged operation on the hardware, it will cause a trap
        - when a trap occurs:
            - application is interrupted
            - cpu hands control over to the os
            - operating system determines what caused the trap
            - os verifies if it should grant the access or terminate the process if the attempt was illegal. this depends on the policies that are supported by the os
- applications can interact with the hardware through system calls
    - the operating system exposes a system call interface that the applicatoin can explicitly invoke if they want the os to perform a service/privileged access on their behalf. examples include
        - open (a file)
        - send (via a socket)
        - mmap (allocate memory)
- the operating system can feed "signals" back to applications, which is a mechanism for the os to pass notifs into the applications

Flow of a synchronous system call

- user process executes
- user process passes its arguments to the system call and makes the system call
- process switches context to kernel/privileged mode, where the system call is executed
    - context switch involves switching to kernel memory where the system call instructions can be executed
- upon completion, system call outputs are returned to the user process address space
    
synchronous system calls are considered expensive operations because

- an application must write arguments
- save all relevant data at a well-defined location before handing off to kernel process
    - this location must be well defined so the os kernel, based on the system call number, can determine the number and location of the arguments
    - arguments can be passed directly between user process and system kernel, or they can be passed indirectly by reference
- make a system call using the specific system call number
- the process waits until the system call completes

the operating system traps application instructions or memory access requiring special privilege. for instance, the application cannot change the contents of certain registers or give itself more cpu or memory. only the os can do that. 

Tradeoffs of system calls: 

- It can take 50-100 ns on a 2Ghz machine running linux to finish the steps involved in a system call - real overhead for the system!
- system call transitions affect the hardware cache usage
    - application performance depends highly on ability to use the hardware cache. accessing the cache requires an order of a few cycles. accessing memory is on the order of a few hundred cycles.
    - when operating system is executing a system call, it will likely bring its contents into the cache, replacing some of the application content that was in the hardware cache. the application would need to access its content from memory, which, as stated before, is on the order of hundreds of times more expensive to access

Examples of system calls:
    - 
    - 
    - 
    - 

Types of operating system design
    - Monolithic
        - everything the operating system could need is included in the box
        - this leads to less maintainability issues, but is more cumbersome (larger operating system size)
    - Modular
        - the operating system contains a few basic services
        - only download the services you need
        - some performance tradeoffs because different services need to be designed with the operating system's interfaces in mind - modularity comes at the cost of some overhead
        - maintainability concerns (service updates can introduce bugs or service versions can be deprecated)
        - commonly used in practice
    - Microkernel
        - at the os level, the microkernel can support some basic services such as address spaces and threads (context) for apps
        - databases, and even file systems and device drivers (commonly included in os), operate at the user/unprivileged level
        - by virtue of this design, these services require a lot of inter process communication (IPC), which the microkernel does support
        - upsides of microkernel are the small, lightweight system size, leading to lower overhead and better performance, and the verifiability/testability of code to behave as it should
        - downside is that it is not very portable because it is customized to the hardware. also introduces complexity because there are different variations of the microkernel for different platforms, meaning some services will need to be custom-written. because more services are in the user/unprivileged layer, they require more interaction with the kernel for inter process communication, which as stated before, is costly

Linux architecture
    - there are several "layers" to the linux architecture. from lowest to highest
        - hardware
        - kernel
            - linux operating system
        - user mode
            - standard library
            - standard utility programs
        - user interface (application)

The kernel itself has several subcomponents, which all have well defined functionality and interfaces. they can be independently modified or replaced, which is what enables modularity
    - i/o component
        - manages the virtual file system, which encompasses
            - terminals
            - character device drivers
            
    - memory management component
        - virtual memory
        - paging page replacement
        - page cache
    - process management component
        - signal handling
        - process/thread creation and termination
        - CPU scheduling

Mac architecture


## Setting up a development environment and testing it
[Source](https://github.gatech.edu/gios-fall-26/environment)
    - this github repo contains a guide explaining several configuration options, their respective starter packages, and articles about using the gdb debugger, remote gdb debugging

Two test frameworks are suggested: munit and cmocka. A quick google search summary described munit as a lightweight unit test framework, and cmocka as a mature, feature rich framework that can also detect memory leaks, buffer overruns, and underruns during test execution. it can also catch and recover from segfaults or illegal instructions mid-test. Cmocka's feature set might take more time to learn up front, but may end up saving more time in the long run. So I chose to download and learn cmocka. It turns out, the API documentation is quite good and there exists some quick startup guides online, so I feel pretty confident about using it. 

I compiled a simple cmocka test harness to test my environment.

## An Introduction to Programming with Threads

[Source](https://s3.amazonaws.com/content.udacity-data.com/courses/ud923/references/ud923-birrell-paper.pdf)

- A thread is a sequential flow of control
- having multiple threads in a program means the program, at any given moment, has multiple instances of execution, one in each of its threads
- programmer must decide when to use multiple threads, and also be aware that the computer may not always execute the threads simultaneously
- having threads execute in the same address space means the threads can access the same section of memory
- thread facilities are advertised to be "lightweight". thread creation, destruction, existence, and synchronization primitives are cheap enough that the programmer can use threads for concurrency needs
- What's the point of concurrency?

    1) modern processors have multi cores. concurrency allows us to take advantage of the available hardware
        
    - the alternative to concurrency is to have the multiple processes to eadch occupy a separate address space of their own, which is costly: it is expensive to set up, and the cost of communicating between different address spaces is high
    - concurrency allows processors to be used cheaply

    2) threads can be assigned to slow devices such as disks, networks, terminals, and printers. when the thread is waiting for input, the computer should be able to do other useful work, so the input and processes are separated in different threads. 
    
    3) Multitasking is also achievable with multiple threads. Each time a user clicks on an application to start different processes, a thread can be used to handle each process. The implementation of this type of application probably also depends on a separate thread to accept mouse click input.
    
    4) Threads can also be used on distributed systems to handle incoming requests. Threads can be used to handle each client's request in parallel instead of serializing them, such as in the case of a file server or spooling print server.
    
    5) Threads can also be used in applications to reduce the latency of operations (time between initiating a procedure and getting the outputs of that procedure). The book gives the example of adding/removing an item from a balanced tree. The operation can be returned to the caller while the tree is rebalanced in a separate thread. This is an example of using concurrency to defer the work of a procedure since the outcome does not depend on it. The benefit of reducing latency to the end user is a more responsive program.

### Designing a Thread Facility

A facility is a construct containing multiple threads accessing some shared resource. Only one thread is allowed to access the resource at a time. This is called asynchronous execution.

- Thread creation
    - A fork is a function that creates a thread by defining the thread's procedure and giving the thread arguments (the necessary ingredients to execute that procedure). it can be thought of as a thread handler
    - The thread "dies" when it returns the results of its procedure
    - a fork returns to its caller a handle/reference to the newly created thread
    - the thread handle can be passed to a join procedure, which waits for the thread to terminate and returns the result of the given thread's initial procedure. join is a synchronization arrangement
    - join is not always used and not always necessary, because threads can communiate their results by some synchronization arrangement other than join

- mutual exclusion (MUTEX)
    - when a thread has access to a shared resource, it locks the resource so no other threads can use it. a MUTEX is a primitive that specifies a particular snippet of code, such as global variables, that only one thread can execute at any time
    - has two states: locked and unlocked
    - a thread executing inside the mutex has a "lock", or is said to "hold the mutex", meaning no other threads can access the mutex. any thread that attempts to access a locked mutex is said to "block" (added to the mutex queue) until the mutex is unlocked
    - a concrete example of a mutex in action is using it to allow only one thread to update a linked list at a time. a mutex and linked list head are declared. then, inside a 'lock' clause, the commands to add a new element to the linked list are executed. this prevents multiple threads from updating the linked list at a time.
    - a mutex can also be used to protect part of a data structure. to express this, the lock clause would begin by selecting the mutex field of the data structure
    - a mutex can be understood as a resource scheduling mechanism. the resource is the shared memory inside the lock clause, and the scheduling policy is one thread at a time

- condition variables - waiting for events
    - a thread has a mutex lock until the lock clause is finished executing, but for more complicated scheduling policies, a condition variable is used
    - a condition is always associated with a mutex and the data protected by that mutex
    - a monitor consists of some data, a mutex, and zero or more condition variables
    - a condition variable has three types of operations: wait, signal, and broadcast, which are triggered by conditions
        - when **wait** is triggered, the thread is blocked (queued) and the mutex is unlocked.
        - the **signal** operation does nothing unless a thread is blocked on the condition variable, in which it unlocks one or more blocked threads
        - **broadcast** awakens all the threads currently blocked on the condition variable
    - when a thread is awoken inside 'wait', it relocks the mutex and returns. unless the mutex is already locked, which will keep the thread blocked on the mutex until it is available
    - if the shared resource is not available, the thread unlocks the mutex and blocks by calling **wait**
    - a thread can call a signal when it is done with its task

- alerts - escaping a thread from a long-term waiting event
    - threads have states
    - one state of a thread is stored in the alert-pending state, which is initially false
    - if AlertWait is triggered, a threads alert-pending is changed from true to false, relocks the mutex, and raises the exception 'alerted'
    - if alertwait is triggered, a thread that was blocked on a condition can be awoken, lock the mutex, and raise the 'alerted' exception
    - calling alert on a thread that is not blocked simply sets its alert-pending boolean to true
    - a testalert call atomically tests and clears the thread's alert-pending boolean
    - example of using an alert: a thread is waiting but a second thread has determined that the first thread no longer needs to wait, the second thread will call Alert(firstThread)

### Using a Mutex: Accessing Shared Data
- unprotected data
    - unsynchronized behavior, or mutating unprotected data with multiple threads, can result in nondeterministic behavior. behavior depends on the precise timing relationship between threads. page faults, the use of real time timer facilities, or asynchrony in a multi-processor system are causes of this nondeterministic behavior. so data must be protected with mutexes
    - "make your mutexes as simple as possible, but no simpler"
- invariants
    - a boolean that is true when the mutexed data is unlocked
    - it's used to describe what the mutex is protecting, when the data protected by the mutex is complicated
    - it's an informalism that helps programmers describe what the mutex is protecting
    - it describes a state about the mutexed data - for example, if a thread updates a mutexed null array at index i, the invariant is that "i is the index of the first null in the array and all elements in the array after i are null". the invariant is false when/after the thread operates on the array, but true before the mutex is locked by a thread
    - threads are responsible for restoring this boolean when finished with the operation, before calling 'wait'
- cheating
    - the idea that you must use a mutex to protect every global variable is based on the model where the actions of the threads are arbitrarily interleaved. there are some instances where this model doesn't hold true, but the conditions are not easily generalized
    - this applies to simple variables as well
    - one "cheat" is to use unsynchronized access as a "hint", knowing that the mutexed operation you really want to call is expensive
        - this cheat only works if the programming language you are using guarantees several assumptions:
            - reaading it without a mutex will not cause a runtime error
            - reading it when its value is False will give you the value False (for example)
- deadlocks involving only mutexes
    - Thread 1 locks mutex A, thread 2 locks mutex B, Thread 1 is blocked on mutex B, thread 2 is blocked on mutex A. so they don't go anywhere because they are stuck in the queue!
    - to solve this, arrange for any thread that has to lock two mutexes to lock them in a specific order every time. for example, this would mean that a thread that needs to lock mutex A and B must lock them in that order
    - a technique that makes this partial order more achievable requires a certain condition. that condition is that the two threads are not modifying the exact same data (for example, thread 1 needs to access the first three elements of an array and thread 2 only needs to access the next three elements). it then becomes possible to place partial locks on the array rather than a lock on the entire array. in the end, requiring that threads lock in a specific order still applies, it's just being applied on a more granular level
        - the tradeoff to this technique is that the locking is more complicated and you are likelier to have unsynchronzied access to shared data. but it is faster if executed correctly
    - having a program deadlock is preferable to having it provide the wrong answer
- poor performance through lock conflicts
    - 
- releasing the mutex within a lock clause

### Using a Condition Variable: Scheduling Shared Resources
- 

### Using Fork: Working in Parallel
- 

### Using Alert: Diverting the Flow of Control
- 

### Additional Techniques
- 

### Building Your Program
- 

### Concluding Remarks
- 