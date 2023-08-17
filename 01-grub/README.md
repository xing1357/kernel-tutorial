#  Booting From GRUB
In this chapter, we look at the assembler, boot from grub and load a basic kernel. We will also learn how to use a linker script, and a Makefile that automates the build.

An operating system comprises of two parts, a Bootloader and Kernel. Luckily, the bootloader we are using is GRUB, which allows us to start at the kernel. However, we need to make sure that certain global variables are set correctly, and a stack is setup before we load the kernel.

## Kernel Entry
The kernel Entry code is the code that is called first when the bootloader calls the kernel. Note, you should know a bit of assembler coming in to this tutorial. The code of the bootloader is very straightforward. We essentially load a 1 MB stack, which will be used to store and pass arguments in C function calls. The `section .multiboot` is the section where GRUB verifies that it is in fact, loading a kernel from the output binary. Finally, in the end we call the "kmain" function. "Later, we will add a GDT and IDT in the assembly file. 

## Linker Script
A linker script is a configuration file used in the process of compiling and linking software, including kernel code. It provides instructions to the linker about how different sections of code and data should be organized in the final executable or binary file. In the context of compiling a kernel, the linker script plays a crucial role in determining how various components of the kernel are placed in memory, their addresses, and how they interact with each other. There are several sections in the linker file, the "text" sections contain the code of your kernel, the data section for hardcoded values, and the bss section for uninitialized data. Note that the "bss" section doesn't exist in the binary output, but exists in memory once the kernel binary is loaded.

## grub.cfg
This is just the configuration file for GRUB to load your kernel, and is pretty self-explanatory

## Makefile
Build the kernel using `make all`, and run the kernel using `make run`. Congratulations, you have booted your first kernel, and should now see a blinking cursor after booting from the grub menu!



