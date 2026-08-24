---
title: GIOS Syllabus Week
date:
  created: 2026-08-23
tags:
  - Notes
  - Operating Systems
---
The first week of GIOS is upon us! Going forward, I will be using these blog posts to document my knowledge as I progress through the class.

Working through the modules:
  - 0.5 - 2 hours of lectures per week
  - 3 projects each with a 3 week timeline

# GIOS Diagnostic Quiz
C concepts to know:
  - Structs, arrays, pointers, and reference types
    - a **struct** is a data structure that contains member variables
    - **arrays** are iterable data structures that store data in contiguous memory
    - **pointers** are references to variables. they are memory addresses, and they are declared by speicfying the type of variable they point to followed by a *. To get the address of a variable, prepend the variable with an ampersand '&'. Dereferencing a null pointer is undefined behavior in C, so here are several ways to address it
      - initialize the pointer immediately
      - perform safe null checks (verify that pointer is not null before extracting its value)
      - set pointers to null after freeing memory. when a pointer's memory is cleared, it becomes a dangling pointer. anything it points to is no longer governed by stack policy.
    - **reference types** Google search claims that there are no "reference types" in C. So this seems like what I discussed in the description about pointers, namely how the pointer variable is declared with the object type of the object it is pointing to
  - File I/O
    - 
  - Use of command line parameters
  - Pass-by-reference and pass-by-value
  - Dynamic memory allocation using malloc()
  - Use of C libraries
  - Debugging programs
  - Reading documentation
  - Iterative design
  - Good coding standards

