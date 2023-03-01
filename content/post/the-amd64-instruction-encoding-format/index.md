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
2. **R﻿EX Prefix** : This is a single byte prefix and there can be maximum one REX prefix in an instruction. REX prefixes are used to extend the register/memory size in an instruction. This was introduced in `AMD64` architecture because the size of `GPRs` (General Purpose Registers) and `EFLAGs` were increased from 32 bits to 64 bits. Value of REX prefix ranges from 0x40 to 0x4f. So to detect whether this prefix is a REX prefix or not, we can check whether upper nibble is 0x4 or not. The lower nibble specifies some other things that we will see later.
3. **V﻿EX/XOP Prefix** : These are again single byte prefixes used to give access to different set of registers. Registers like `XMM0`, `XMM1`, ...., `XMM15`, `YMM0`, `YMM1`, ..., `YMM15`, `MMX0`, `MMX1`, ..., `MMX15` and `ZMM0`, `ZMM1`, ..., `ZMM15`. This means a different class of instructions too (Vectorized).

## O﻿pcode Bytes

A﻿n opcode byte is a special byte that uniquely identifies an instruction. For example `0x90` is a special byte that uniquely identifies the `nop` instruction. Also notice that this instruction won't need any of the prefix bytes! To uniquely identify an instruction there can be maximum of three opcode bytes and for these we have three different opcode maps. For instructions that have more than three opcode bytes, we have escape sequences.

## E﻿scape Sequences

A﻿n escape sequence is a unique set of bytes (or a just a single byte) that adds more variety of instructions to the basic instruction set. For example, instructions that have two opcode bytes have `0x0f` as their first opcode byte as escape sequence.

## T﻿he ModRM Byte

T﻿he ModRM byte has three fields.

* M﻿ode field (upper two bits, 0xc0 mask)
* R﻿ field (next three bits, 0x38 mask)
* R﻿M field (lower three bits, 0x07 mask)

![](screenshot-from-2023-03-01-19-02-58.png "Diagram of the ModRM byte (AMD Vol3 Page17)")

### T﻿he Legacy Prefixes

L﻿egacy prefixes are of five types and each type has specific byte assigned. Comparing the first few bytes of instruction with these special bytes, we can check whether this byte is a legacy prefix byte or not.

![(AMD Vol3 Page7)](screenshot-from-2023-03-01-18-42-05.png "Table showing all legacy prefix bytes with their meaning and names (AMD Vol3 Page7)")