# RISC-V Assembly
## Goals

- Get familiar with using the Venus simulator
- Get an idea of how to translate C code to RISC-V.
- Write RISC-V functions with the correct function calling procedure.

## Introduction

These exercises focus of the RISC-V assembly language, which is a lower-level language much closer to machine code. We can't execute RISC-V code directly because your computer is built to run machine code from other assembly languages --- most likely Intel or ARM.

For these labs, we will work with several RISC-V assembly files, each of which has a .s file extension. To run these, we will be using Venus, an educational RISC-V assembler and simulator. You can run Venus on the [Venus website](https://venus.cs61c.org/) or using the [extention](https://marketplace.visualstudio.com/items?itemName=hm.riscv-venus) for [Visual Studio Code](https://code.visualstudio.com/). You can find the documentation of both versions ([website](https://cs61c.org/sp22/resources/venus-reference/), [extention](https://github.com/hm-riscv/vscode-riscv-venus)), but personally, we find the second version more conventional yet functional. 