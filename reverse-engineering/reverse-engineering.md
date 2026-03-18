# Binaries

## In computing, binaries are files compiled from source code. For example, you run a binary when launching an executable file on your computer. At one point in time, this application would've been programmed in a programming language such as C#. It is then compiled, and the compiler translates the code into machine instructions

## Binaries have a specific structure depending on the operating system they are designed to run. For example, Windows binaries follow the Portable Executable (PE) structure, whereas on Linux, binaries follow the Executable and Linkable Format (ELF). This is why, for example, you cannot run a .exe file on MacOS. With that said, all binaries will contain at least

- A code section: This section contains the instructions that the CPU will execute
- A data section: This section contains information such as variables, resources (images, other data), etc
- Import/Export tables: These tables reference additional libraries used (imported) or exported by the binary. Binaries often rely on libraries to perform functions. For example, interacting with the Windows API to manipulate files

[Day 17 AoC -2](https://tryhackme.com/room/adventofcyber2)

## Introduction to x86-64 Assembly

Computers execute machine code, which is encoded as bytes, to carry out tasks on a computer. Since different computers have different processors, the machine code executed on these computers is specific to the processor. In this case, we’ll be looking at the Intel x86-64 instruction set architecture which is most commonly found today. Machine code is usually represented by a more readable form of the code called assembly code. This machine is code is usually produced by a compiler, which takes the source code of a file, and after going through some intermediate stages, produces machine code that can be executed by a computer.

Without going into too much detail, Intel first started out by building a 16-bit instruction set, followed by 32 bit, after which they finally created 64 bit. All these instruction sets have been created for backward compatibility, so code compiled for 32-bit architecture will run on 64-bit machines. As mentioned earlier, before an executable file is produced, the source code is first compiled into assembly(.s files), after which the assembler converts it into an object program(.o files), and operations with a linker finally make it an executable.

The best way to actually start explaining assembly is by diving in. We’ll be using radare2 to do this - [radare2](./r2book.pdf) is a framework for reverse engineering and analysing binaries. It can be used to disassemble binaries(translate machine code to assembly, which is actually readable) and debug said binaries(by allowing a user to step through the execution and view the state of the program).

The resources that can be used to learn radre2 are below:
    1. [Radre2 cheatsheet](./r2_cs.pdf)
    2. [Radre2 book](./r2book.pdf)
    3. [Radre2 tutorial](./radare2_tutorial.pdf)
