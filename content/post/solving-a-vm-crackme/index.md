---
title: Solving a VM CrackMe
subtitle: Beginners guide to solving a VM crackme.
date: 2021-12-24T12:48:01.553Z
draft: false
featured: false
tags:
  - reversing
  - virtual-machine
  - crackme
  - beginner
categories:
  - virtual-machine
  - reversing
  - crackme
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
I'm always having problems solving a VM obfucscation challenge in any CTF. This time I plan to end this by solving a VM CrackMe. I got this from a team-mate ( [h4x5p4c3](https://twitter.com/h4x5p4c3) ), another helpful team-mate.

Here are a few resources before we begin : 

* [Analysis Of Virtualization-based Obfuscation](https://youtu.be/b6udPT79itk)
* [The challenge](https://drive.google.com/file/d/1Yc54_ogPcVUpFICXVPOnFLsGvS8Xq668/view?usp=sharing)

As I mentioned earlier, I am not good with VM crackmes but when I started following that video (first), just 40 minutes into the video and I'm able to detect the dispatcher function. I highly recommend anyone who really wants to learn. I'll be solving a few VM crackmes to improve my skills. I have basic reversing skills but I have a bad habit of getting stuck in useless functions or parts of code like we did with XVM. Our goal was to solve the crackme and I started to reconstruct the whole code. I won't be doing such things in this one. Although we might go into detail.

At the time of writing I'm only 52 minutes into that [video](https://www.youtube.com/watch?v=b6udPT79itk&t=3278s) and still I was able to solve this challenge. Maybe the challenge is easier but I also feel that the video is effective too. 

I've already partially solved this crackme and this'll be a detailed explanation on my struggles and all. Recently we participated in xmas ctf and I really felt like an impostor. I have two options : either I fight this and learn all about VMs (well, most of the stuffs) or I can just stay an impostor. I'll walk on the path less travelled by.

## What Is A VM?

VM is short form of Virtual Machine. A virtual machine is exactly what it's name is, it's virtual!.

An example of physical machine is your CPU. Your CPU is executing real instructions. When you do a `mov` instruction, your CPU will take minimum number of steps to complete that instruction. That instruction will have effect on real registers and memory. 

When it comes to a virtual CPU (machine), it may or may not have a move instruction in the first place! Even if it has a mov instruction, it'll be moving data to/from variables declared within the program. So the virtual cpu doesn't directly use anything real (registers/memory) and all the resources that it'll use will be stored in a variable that is already allocated on either the stack or the heap. All the (virtual) instructions that a virtual machine will run will ultimately be converted to machine code. Because of this conversion, a virtual CPU takes much more number of steps than a physical CPU to execute a single (virtual) instruction and because of this, virtual machines are slower too.

## How Do Virtual Machines Work?

Since a virtual machine is trying to emulate some new instruction set, it'll need to have a CPU that will be able to decode those set of instructions and for that all virtual machines implement their own virtual CPU. How do we do that? Well, a CPU is just a bunch of registers and some helper units.

![](abasiccomputer.gif)

A virtual CPU is much similar to a physical CPU. It'll have it's own set of registers and cache and all. A simple implementation in code will look something like this

```cpp
enum reg_id { 
    rax = 0, rbx, rcx, ...
}

struct cpu_t{
    int64_t registers[X]; // X number of registers
    int64_t* stack;
  
    size_t pc; // program counter register
    uint32_t* bytecode;
    size_t code_sz;
    .
    .
    .
    // some other things needed for this vm
};
```

This makes up the structure of our CPU, but then how will it execute instructions? Normally, challenge developers write their program in a symbolic language and then convert it to the virtual CPUs assembly code. This assembled code is what we refer to as the bytecode. It's just an array of numbers like the opcodes of your actual CPU.

This bytecode is then read and passed to a **Fetch**, **Decode**, **Execute** (FeDeX) loop. This loop does what it's name is. It fetches the current instruction from bytecode, decodes what it means (useful part) and then executes it! This is very much similar to what an actual CPU does but this decoding part makes up the extra steps that our virtual CPU has to take.

## The FeDeX Loop

In normal CPU, there is a register called the **program counter** (instruction pointer in Intel CPUs). This register contains an absolute address or an offset from a base address that directly or indirectly points to the next instruction that needs to be executed. So, the fetch part just needs to keep track of this program counter. To fetch the next instruction, the program will do

```c
enum instr{
    MOV_REG_CONST = 0, // mov const value to register
    MOV_REG_REG, // move value in register to register
    ADD_REG_CONST, // add value in register to a const value and store in register
    .
    .
    .
}

void dispatch(){
    while(cpu.pc < cpu.code_sz){
         // fetch current instruction and increment pc
         uint32_t current_instr = cpu.bytecode[cpu.pc++];
         
         // decode
         if(current_instr == MOV_REG_CONST){
             // execute
             uint32_t reg_idx = cpu.bytecode[cpu.pc++];
             uint32_t const_val = cpu.bytecode[cpu.pc++];
             cpu.register[reg_idx] = const val;
         }else if(current_instr == MOV_REG_REG){
             // execute
         }
         .
         .
         .
    }
}
```

Instead of if-else statements, switch-case statements can be used too! Now, this is where the interesting part comes : In all normal, not crazy VMs you'll find a FeDeX loop like this one.  This function is what we need to find if it's a VM challenge. Generally it's easy to find because of the code structure here! When you'll see this code in a graph like format, you'll notice that almost all similar VMs (with a FeDeX loop) will have similar structure. Let's try that in this crackme.

## First Contact

We'll a new tool **Iaito**. Iaito is the GUI version of radare. We'll use this to quickly get to the dispatcher function and then we can switch to any other tool.

![](entry.png)

This is doing a simple call to `libc_start_main` which will eventually call `main`.

> At this point I'd like to highlight that reversing XVM from scratch wasn't a complete waste as I can do normal reversing like this very easily. I can now read the assembly very easily and understand what the code is trying to do!

Instead of going to main and following the code sequentially, we'll now go through all those functions and check the graph structure from a high level perspective.

![](main.png "high level view of main")

Simple and short. The head block is big and is most probably reading input and doing some initializations. On going through functions one by one, you'll see a function like this

![](overview.png)

Looks like this one has lots of conditional jumps. Let's analyse this one. Spoiler : This is is indeed the dispatcher function.

## Analysis

Now we can go to main and look where this function is being called and then analyze what's being passed to this. We'll try to do as less reversing as possible in functions outside this one. I'll name this function to *dispatcher* to increase readability.

![](2.png "call to dispatcher in main")

Clearly this is taking three arguments, first one is some value in `rax`, second value looks like some address, and third one is some number. Let's check the value in rax.

![](1.png)