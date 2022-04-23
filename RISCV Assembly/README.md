# RISC-V Assembly
## Goals

- Get familiar with using the Venus simulator
- Get an idea of how to translate C code to RISC-V.
- Write RISC-V functions with the correct function calling procedure.

## Introduction

These exercises focus of the RISC-V assembly language, which is a lower-level language much closer to machine code. We can't execute RISC-V code directly because your computer is built to run machine code from other assembly languages --- most likely Intel or ARM.

For these labs, we will work with several RISC-V assembly files, each of which has a .s file extension. To run these, we will be using Venus, an educational RISC-V assembler and simulator. You can run Venus on the [Venus website](https://venus.cs61c.org/) or using the [extention](https://marketplace.visualstudio.com/items?itemName=hm.riscv-venus) for [Visual Studio Code](https://code.visualstudio.com/). You can find the documentation of both versions ([website](https://cs61c.org/sp22/resources/venus-reference/), [extention](https://github.com/hm-riscv/vscode-riscv-venus)), but personally, we find the second version more conventional yet functional. 

## Exercise 1: Get familiar with Venus

In this exercise, we are giving you the implementation of fib in both c and assembly. Take a look at both files. You also may need to check [RISC-V-Reference](https://didatticaonline.unitn.it/dol/mod/resource/view.php?id=1050250) and [RISC-V Calling Converntion](https://didatticaonline.unitn.it/dol/mod/resource/view.php?id=1050253).

At the top of `fib.s`, you can see the `.data`, `.word`, `.text` directives.

- `.data`: Denotes where global variables are declared
- `.word`: Allocates and initializes space for a 4-byte variable in the data segment.
- `.text`: Indicates the start of the code.

We have added comments to `fib.s` to help you understand the program. There are two new instructions:

- `la n`: loads the address of the label where `n` is located. It is a pseudo instruction that breaks down into the instructions `auipc` and `addi`. Take a look at your reference card to see what `auipc` does.
- `ecall`: The ecall instruction is used to perform system calls or request other privileged operations such as accessing the file system or writing output to console. In this class, we will mostly be using ecall to exit or to print integers. To specify which action the `ecall` should take, you will pass a code to `ecall` through `a0`. To terminate the program, you set `a0` to `10`. To print an integer, you will set `a0` to `1` and set `a1` to the integer that you want to print.

### Quiz

Try to run `fib.s` with Venus and answers the following questions.

1. Run the program to completion. What number did the program output? What does this number represent?
2. At what address is `n` stored in memory? Hint: Step through the code and look at the contents of the registers.
3. How would you compute the 20th fibonacci number?

## Exercise 2: From C to RISC-V

Open the files `sum.c` and `sum.s`. The assembly code provided (`.s` file) is a translation of the given C program into RISC-V.

### Quiz

Find and identify the following components of this assembly file, and be able to explain how they work.

1. The register representing the variable `k`.
2. The register representing the variable `sum`.
3. The registers acting as pointers to the `source` and `dest` arrays.
4. The assembly code for the loop found in the C code.
5. How the pointers are manipulated in the assembly code.