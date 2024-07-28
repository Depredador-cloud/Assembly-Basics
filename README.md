# Assembly-Basics

Here's a detailed assembly guide in Markdown format for your GitHub repository. This guide covers the basics of assembly language, including setup, essential commands, and examples from different architectures.

---

# Assembly Language Programming Guide

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Setting Up Your Environment](#setting-up-your-environment)
4. [Basic Concepts](#basic-concepts)
5. [x86-64 Assembly Language](#x86-64-assembly-language)
6. [ARM Assembly Language](#arm-assembly-language)
7. [AVR Assembly Language](#avr-assembly-language)
8. [8051 Assembly Language](#8051-assembly-language)
9. [Example Programs](#example-programs)
10. [Debugging](#debugging)
11. [Resources](#resources)

## Introduction

Assembly language is a low-level programming language that provides a direct interface to the hardware. It is specific to a particular computer architecture and provides a way to write programs that are closely aligned with the machine's instruction set.

## Prerequisites

Before diving into assembly language programming, it is recommended that you have a basic understanding of:

- A high-level programming language (e.g., C, C++, Java)
- Computer architecture concepts
- Command line operations in Linux

## Setting Up Your Environment

To get started with assembly language programming, you need to set up your development environment. Here are the steps for setting up an environment for x86-64, ARM, AVR, and 8051 architectures.

### x86-64 Assembly on Ubuntu

1. **Install Development Tools:**
    ```bash
    sudo apt update
    sudo apt install build-essential gdb nasm
    ```

2. **Writing Your First Program:**
    ```asm
    ; hello.asm
    section .data
    hello db 'Hello, world!', 0

    section .text
    global _start

    _start:
        mov rax, 1
        mov rdi, 1
        mov rsi, hello
        mov rdx, 13
        syscall

        mov rax, 60
        xor rdi, rdi
        syscall
    ```

3. **Assembling and Running:**
    ```bash
    nasm -f elf64 hello.asm -o hello.o
    ld hello.o -o hello
    ./hello
    ```

### ARM Assembly

1. **Install ARM Development Tools:**
    ```bash
    sudo apt update
    sudo apt install gcc-arm-none-eabi gdb-multiarch
    ```

2. **Writing Your First Program:**
    ```asm
    ; hello.s
    .section .data
    hello: .asciz "Hello, world!\n"

    .section .text
    .global _start

    _start:
        ldr r0, =hello
        bl printf

        mov r7, #1
        svc 0
    ```

3. **Assembling and Running:**
    ```bash
    arm-none-eabi-gcc -nostartfiles -Ttext=0x10000 -o hello hello.s
    qemu-arm -L /usr/arm-linux-gnueabi/ hello
    ```

### AVR Assembly

1. **Install AVR Development Tools:**
    ```bash
    sudo apt update
    sudo apt install avr-gcc avr-libc avrdude
    ```

2. **Writing Your First Program:**
    ```asm
    ; blink.asm
    .include "m16def.inc"

    .cseg
    .org 0x00
    rjmp reset

    reset:
        ldi r16, 0xFF
        out DDRB, r16

    loop:
        ldi r16, 0xFF
        out PORTB, r16
        rcall delay
        ldi r16, 0x00
        out PORTB, r16
        rcall delay
        rjmp loop

    delay:
        ldi r17, 0xFF
    delay_loop:
        dec r17
        brne delay_loop
        ret
    ```

3. **Assembling and Uploading:**
    ```bash
    avr-as -mmcu=atmega16 -o blink.o blink.asm
    avr-ld -o blink.elf blink.o
    avrdude -c usbtiny -p m16 -U flash:w:blink.elf
    ```

### 8051 Assembly

1. **Install 8051 Development Tools:**
    ```bash
    sudo apt update
    sudo apt install sdcc
    ```

2. **Writing Your First Program:**
    ```asm
    ; blink.a51
    ORG 0H
    MOV P1, #0FFH

    AGAIN:
        MOV A, P1
        CPL A
        MOV P1, A
        ACALL DELAY
        SJMP AGAIN

    DELAY:
        MOV R7, #255
    DELAY_LOOP:
        DJNZ R7, DELAY_LOOP
        RET
    ```

3. **Assembling and Uploading:**
    ```bash
    sdcc -mmcs51 --out-fmt-ihx blink.a51
    stcgal -p /dev/ttyUSB0 blink.ihx
    ```

## Basic Concepts

### Registers

Registers are small, fast storage locations within the CPU used to perform operations. Common registers include:

- General Purpose Registers (GPRs)
- Stack Pointer (SP)
- Base Pointer (BP)
- Instruction Pointer (IP)
- Flags Register

### Memory Segments

- **Data Segment:** Stores global and static variables.
- **Code Segment:** Stores the executable instructions.
- **Stack Segment:** Stores function parameters, return addresses, and local variables.

### Instructions

Instructions are the commands executed by the CPU. Common instructions include:

- **MOV:** Transfer data between registers.
- **ADD:** Add two values.
- **SUB:** Subtract one value from another.
- **MUL:** Multiply two values.
- **DIV:** Divide one value by another.
- **JMP:** Jump to a specified address.
- **CALL:** Call a subroutine.
- **RET:** Return from a subroutine.

## x86-64 Assembly Language

Refer to [x86-64 Assembly Language Programming with Ubuntu](#10) for detailed information on x86-64 architecture and instructions.

## ARM Assembly Language

Refer to [Modern Assembly Language Programming with the ARM Processor](#10) for detailed information on ARM architecture and instructions.

## AVR Assembly Language

Refer to [Some Assembly Required: Assembly Language Programming with the AVR Microcontroller](#10) for detailed information on AVR architecture and instructions.

## 8051 Assembly Language

Refer to [The 8051 Microcontroller and Embedded Systems Using Assembly and C](#10) for detailed information on 8051 architecture and instructions.

## Example Programs

### x86-64 Example: Fibonacci Sequence

```asm
section .data
    fib db 0, 1, 0

section .text
global _start

_start:
    mov rsi, fib
    mov rax, [rsi]
    mov rbx, [rsi + 1]

loop:
    add rax, rbx
    mov [rsi + 2], rax
    mov rax, rbx
    mov rbx, [rsi + 2]
    add rsi, 1
    cmp rsi, fib + 20
    jne loop

    mov rax, 60
    xor rdi, rdi
    syscall
```

## Debugging

### Using GDB

GDB is a powerful debugger for tracking down bugs in your assembly code. To use GDB:

1. **Compile with Debug Info:**
    ```bash
    nasm -f elf64 -g -F dwarf hello.asm -o hello.o
    ld hello.o -o hello
    ```

2. **Start GDB:**
    ```bash
    gdb ./hello
    ```

3. **Common GDB Commands:**
    - `break _start`: Set a breakpoint at the start.
    - `run`: Run the program.
    - `step`: Step through the code.
    - `print $rax`: Print the value of the RAX register.
    - `info registers`: Display all registers.

## Resources

- **Books:**
    - "Modern X86 Assembly Language Programming"
    - "Low-Level Programming: C, Assembly, and Program Execution on Intel® 64 Architecture"
    - "Assembly Language Step-By-Step: Programming with Linux"

- **Online Tutorials:**
    - [TutorialsPoint - Assembly Programming](https://www.tutorialspoint.com/assembly_programming/index.htm)
    - [Learn Assembly - Assembly Language Online Course](https://www.learnassembly.com/)

---

This guide provides a solid foundation to start learning and practicing assembly language programming across different architectures. Happy coding!
