
# Analyzing Helloworld.c (WORK IN PROGRESS)

## Table of Contents
* [Introduction](#introduction)
* [C Code](#c-code)
* [ASM Code](#asm-code)
* [Ghidra Analysis](#Ghidra-Analysis)
* [IDA Pro Analysis](#IDA-Pro-Analysis)


## Introduction

## C Code

We have all seen it before, but here it is again, helloworld.c...

```
#include <stdio.h>

int main(int argc, char* argv[])
{
    printf("Hello world!\n");
    
    return 0;
}
```
## ASM Code

Assembled into assembly (x86-64), that looks something like this.

```
~/REPractice/helloworld$ gcc -S helloworld.c
```

```
	.file	"helloworld.c"
	.text
	.section	.rodata
.LC0:
	.string	"Hello world!"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp				; Frame pointer setup
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$16, %rsp

	; Start of the important part (main)

	movl	%edi, -4(%rbp)		; argc
	movq	%rsi, -16(%rbp)		; argv
	leaq	.LC0(%rip), %rdi	
	; Loading the effective address of string consant .LC0 (declared up top as hello world)
	; Loaded into RDI
	call	puts@PLT			; printf call (Procedure Linkage Table)
	movl	$0, %eax			; Set EAX to 0 since we are returning 0
	leave
	.cfi_def_cfa 7, 8
	ret							; return
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	 1f - 0f
	.long	 4f - 1f
	.long	 5
0:
	.string	 "GNU"
1:
	.align 8
	.long	 0xc0000002
	.long	 3f - 2f
2:
	.long	 0x3
3:
	.align 8
4:

```

## Ghidra Analysis

Let us see if there is a difference between what we get in Ghidra, vs what the Assembled Code actually was.

```
                             **************************************************************
                             *                          FUNCTION                          *
                             **************************************************************
                             undefined main()
             undefined         AL:1           <RETURN>
             undefined4        Stack[-0xc]:4  local_c                                 XREF[1]:     00101155(W)  
             undefined8        Stack[-0x18]:8 local_18                                XREF[1]:     00101158(W)  
                             main                                            XREF[4]:     Entry Point(*), 
                                                                                          _start:00101081(*), 0010203c, 
                                                                                          001020e8(*)  
        00101149 f3 0f 1e fa     ENDBR64
        0010114d 55              PUSH       RBP
        0010114e 48 89 e5        MOV        RBP,RSP
        00101151 48 83 ec 10     SUB        RSP,0x10
        00101155 89 7d fc        MOV        dword ptr [RBP + local_c],EDI
        00101158 48 89 75 f0     MOV        qword ptr [RBP + local_18],RSI
        0010115c 48 8d 3d        LEA        RDI,[s_Hello_World_00102004]                     = "Hello World"
                 a1 0e 00 00
        00101163 e8 e8 fe        CALL       <EXTERNAL>::puts                                 int puts(char * __s)
                 ff ff
        00101168 b8 00 00        MOV        EAX,0x0
                 00 00
        0010116d c9              LEAVE
        0010116e c3              RET

```

## Breakdown



