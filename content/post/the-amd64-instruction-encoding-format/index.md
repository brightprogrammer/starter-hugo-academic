---
title: The AMD64 Instruction Encoding Format
subtitle: Understanding The AMD64 Instruction Encoding Format
date: 2023-03-01T13:14:02.601Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
For past few days I've been working on a personal program/binary analysis tool which I'm calling **Katana**. I've made a simple Elf file loader for now but for it to have more utilities while working, and for me to learn something more, I've started working on my own disassembler for Katana. For now I want to support only `AMD64` and `x86` Instruction formats but in future I want to add support for `ARM`, `MIPS`, `RISCV` etc... I've also been doing live streams but the progress is really really slow! I have no guidance and I'm just reading the manuals and working on my own which is quite difficult. But! I won't complain and I think I've found a solution for this! and the solution is to write a blog on the instruction encoding format so that this will act like notes for me.

## A﻿rchitecture Programmer Manual

T﻿his manual describes all the workings on an AMD64 CPU, like it's an interface. Whatever's in the manual must be (assumed) to be true because it is provided by the CPU vendor! T﻿here are two versions of AMD64 architecture programmer manual. One is given by Intel and other my AMD itself. I personally find AMD much easier to read because it has less content, better diagrams and it's precise. Other's might find Intel to be more precise and complete. Here are the two versions :

* [Intel® 64 and IA-32 Architectures Software Developer’s Manual Combined Volumes: 1, 2A, 2B, 2C, 2D, 3A, 3B, 3C, 3D, and 4](https://cdrdv2.intel.com/v1/dl/getContent/671200)
* [amd64-architecture-programmers-manual-volumes-1-5](https://www.amd.com/system/files/TechDocs/40332.pdf)

P﻿lease keep taking reference from this while reading this blog post. But before I can tell you anything about instruction encoding, I have to instroduce you to some basic terms that will be used a lot.

## P﻿refixes

A﻿ prefix is a byte that comes at the very beginning of instruction. All prefixes are optional! This means there are instructions that don't have prefixes at all! They are used to modify the basic instruction's register/memory size, change segment's being used, change behaviour by making the instruction repeat multiple times (in string instructions) and give it other magical powers!

T﻿here are three types of prefixes :

1. **L﻿egacy Prefixes** : These prefixes were present in older architectures also (before `AMD64`). There can be maximum of four legacy prefix bytes in an instruction. There are five different types of legacy prefixes.
2. **R﻿EX Prefix** : This is a single byte prefix and there can be maximum one REX prefix in an instruction. They allow access to R-Extended and new registers added to AMD64 architecture. Value of REX prefix ranges from 0x40 to 0x4f. So to detect whether this prefix is a REX prefix or not, we can check whether upper nibble is 0x4 or not.

   ![Fig 1-3 (AMD Vol3 Page15)](screenshot-from-2023-03-01-23-21-21.png "Layout of REX prefix byte (AMD Vol3 Page15)")
3. **V﻿EX/XOP Prefix** : I'll cover this in a later part maybe, ignore this for now. 

I﻿ will explain legacy prefixes in more detail later, for now let's just skim through each name so that you have the names in mind and have an abstract idea of what they do.

## O﻿pcode Bytes

A﻿n opcode byte is a special byte that uniquely identifies an instruction. For example `0x90` is a special byte that uniquely identifies the `nop` instruction. Also notice that this instruction won't need any of the prefix bytes! To uniquely identify an instruction there can be maximum of three opcode bytes and for these we have three different opcode maps. For instructions that have more than three opcode bytes, we have escape sequences.

## E﻿scape Sequences

A﻿n escape sequence is a unique set of bytes (or a just a single byte) that adds more variety of instructions to the basic instruction set. For example, instructions that have two opcode bytes have `0x0f` as their first opcode byte as escape sequence.

## T﻿he ModRM Byte

T﻿he ModRM byte has three fields.

* M﻿ode field (upper two bits, 0xc0 mask)
* R﻿ field (next three bits, 0x38 mask)
* R﻿M field (lower three bits, 0x07 mask)

![(AMD Vol3 Page17)](screenshot-from-2023-03-01-19-02-58.png "Diagram of the ModRM byte (AMD Vol3 Page17)")

### The `M﻿odRM.mod` Field

T﻿he `ModRM.mod` field is short for form Mode or to be more precise, it stands for different **Register Addressing Modes**. A register addressing mode, tells us whether the operands stay in the register or in a memory. Any instruction that takes operands, needs it's operands to exist somewhere! This can be in either registers (meaning all operands are registers) or atleast one of the operand is a memory operand. We know that there can't be instructions where all operands (both in case of instructions that take two operands) are memory operands. This means there are two types of addressing modes : 

* **R﻿egister Direct Addressing Mode** (`ModRM.mod = 11b`) :  All operands are in registers.
* **R﻿egister Indirect Addressing Mode** (`ModRM.mod != 11b`) : Some operands are in registers and some in memory. In case of instructins that take two operands, there will be one operand in a memory location somewhere and other will be in a register.

E﻿xamples of instructions in register-direct addressing mode : 

* `x﻿or eax, eax`
* `a﻿dd eax, ebx`
* `p﻿op ecx`
* `a﻿nd rax, rbx`
* `m﻿ov eax, 0`

E﻿xamples of instructions in register-indirect addressing mode : 

* `x﻿or eax, [eax]`
* `a﻿dd dword ptr ds:[eax], ebx`
* `mov byte ptr ds:[eax*4 + 0xcafebabe], ch`
* `m﻿ov qword ptr[rbp-0x08], 0`

When using indirect addressing mode, we will optionally need a displacement value and optionally an **SIB (Scale Index Base)** byte. The task of SIB byte is to help us iterate over array like memory regions, given a base value in displacement field, an index register where the index of entry in the array will be stored and a scale value that can be 1, 2, 4 or 8. More on SIB byte later.

### T﻿he `ModRM.r` Field

T﻿his specifies the register being used in the instruction. Three bits means total of 8 different values. Remind me, how many GPRs are there in x86 instruction set? (`eax`, `ebx`, `ecx`, `edx`, `esi`, `edi`, `esp`, `ebp`). Total 8 right? How do we access the extended `rXX` counterparts of these registers in `AMD64` architecture? Take your time and recall what you've learned up until now in this post. The answer is by using `REX` prefix. There's a specific bit (`REX.W`) in the lower nibble of `REX` prefix that is used in combination to this `ModRM.r` field and another bit in `REX` (`REX.B`) to allow it 16 different registers . If `REX.R` is zero then you won't be able to access the complete R-Extended registers but instead lower parts like lower double-word or just a word. The `REX.B` bit is placed as the most significant bit in front of `ModRM.r` field to create the whole field of 4 bits and hence get 16 different values to access 16 different GPRs.

### T﻿he `ModRM.rm` Field

T﻿his specifies the register/memory operand based on the register addressing mode we talked about. If the mode is register direct addressing mode then this will refer to a register, else a memory operand. Here instead of `REX.W` and `REX.R`, there is `REX.X` and `REX.B` respectively. The rest of the working is pretty much same as `ModRM.r` field except a few places that we will discuss later.

## T﻿he Combination Of `REX` and `ModRM` Byte

A﻿s I mentioned, `REX` byte is used in combination with `ModRM` byte to give access to all 16 R-Extended registers. Before we continue, I'd like to give you values of the `ModRM.r` and `ModRM.r/m` field that index different registers.

![Table 1-10 (AMD Vol3 Page18)](screenshot-from-2023-03-01-23-32-13.png "Table showing different values of ModRM fields to index different registers (AMD Vol3 Page18)")

P﻿lease ignore the fourth column for now and only refer to first, second and third one. For given 8 values using only `ModRM.r` or `ModRM.rm` field and no `REX` prefix byte, we can have access to only these registers, but if we flag the `REX.R` bit as 1 then we 8 more different values which are used to index registers `r8` to `r15`. There are instructions that access only a part of these registers, like only the lower byte, or word, or double word. In those cases the `REX.W` prefix will be ignored. `REX.W` prefix is taken into account only where it's possible to increaase the size of registers to their maximum.

![Figure 2-3 (AMD Vol3 Page39)](screenshot-from-2023-03-01-23-39-34.png "Table displaying the GPRs and their format (AMD Vol3 Page39)")

### T﻿he Legacy Prefixes

L﻿egacy prefixes are of five types and each type has specific byte assigned. Comparing the first few bytes of instruction with these special bytes, we can check whether this byte is a legacy prefix byte or not.

![Table 1-1 (AMD Vol3 Page7)](screenshot-from-2023-03-01-18-42-05.png "Table showing all legacy prefix bytes with their meaning and names (AMD Vol3 Page7)")