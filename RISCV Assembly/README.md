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

## Exercise 3: Factorial

In this exercise, you will be implementing the `factorial` function in RISC-V. This function takes in a single integer parameter `n` and returns `n!`. A stub of this function can be found in the file `factorial.s`.

The argument that is passed into the function is located at the label `n`. You can modify `n` to test different factorials. To implement, you will need to add instructions under the `factorial` label. Note that you may find it helpful to add additional labels to simplify control flow. We recommend that you implement the iterative solution, but you are welcome to implement the recursive solution. You can assume that the `factorial` function will only be called on positive values with results that won't overflow a 32-bit two's complement integer.

### Action

Complete the code at the comment line `# YOUR CODE HERE` and make sure that your function properly returns, for example, `3! = 6`, `7! = 5040`, and `8! = 40320`.

## Exercise 4: Call a RISC-V function with `map`

This exercise uses the file `list_map.s`.

In this exercise, you will complete the implementation of [map](https://en.wikipedia.org/wiki/Map_(higher-order_function)) on linked lists in RISC-V. The function will be simplified to mutate the list in-place, rather than creating and returning a new list with the modified values.

You will find it helpful to refer to the [RISC-V-Reference](https://didatticaonline.unitn.it/dol/mod/resource/view.php?id=1050250) to complete this exercise. If you encounter any instructions or pseudo-instructions you are unfamiliar with, use this as a resource.

Your `map` procedure will take two parameters. The first parameter will be **the address of the head node** of a singly linked list whose values are 32-bit integers. So, in C, the structure would be defined as:

```c
struct node {
    int value;
    struct node *next;
};
```

The second parameter will be **the address of a function** that takes one `int` as an argument and returns an `int`. We'll use the `jalr` RISC-V instruction to call this function on the list node values (check yourself how `jalr` works).

The `map` function will recursively go down the list, applying the function to each value of the list and storing the value returned in that corresponding node. In C, the function would be something like this:

```c
void map(struct node *head, int (*f)(int)) 
{
    if (!head) { return; }
    head->value = f(head->value);
    map(head->next,f);
}
```

If you haven't seen the int `(*f)(int)` kind of declaration before, don't worry too much about it. Basically, it means that `f` is a pointer to a function that takes an `int` as an argument. We can call this function `f` just like any other.

There are exactly ten (10) markers (1 in `done`, 7 in `map`, and 2 in `main`) in the provided code where it says `# YOUR CODE HERE`.

### Action

Complete the implementation of `map` by filling out each of these ten markers with the appropriate code. Furthermore, provide a call to `map` with `square` as the function argument. There are comments in the code that explain what should be accomplished at each marker. When you've filled in these instructions, running the code should provide you with the following output:

```
9 8 7 6 5 4 3 2 1 0 
81 64 49 36 25 16 9 4 1 0 
80 63 48 35 24 15 8 3 0 -1 
```

The first line is the original list, and the second line is the list with all elements squared after calling `map(head, &square)`, and the third is the list with all elements decremented after now calling `map(head, &decrement)`.

Do not forger to check that your code satisfies the [RISC-V Calling Converntion](https://didatticaonline.unitn.it/dol/mod/resource/view.php?id=1050253), so make sure you are saving and loading where necessary.

## Exercise 5: Array Practice

Consider the discrete-valued function f defined on integers in the set {-3, -2, -1, 0, 1, 2, 3}. Here's the function definition:

```
f(-3) = 6
f(-2) = 61
f(-1) = 17
f(0) = -38
f(1) = 19
f(2) = 42
f(3) = 5
```

### Action

Implement the function in `discrete_fn.s` in RISC-V, with the condition that your code may **NOT** use any branch and/or jump instructions! We have provided some hints in case you get stuck.

Note that you can shorten `jal ra, label` to `jal label`. These two lines do the same thing.
<details>
    <summary>Hint 1</summary>
    <br>
    All of the output values are stored in the output array which is passed to <code>f</code> through register <code>a1</code>. You can index into that array to get the output corresponding to the input.
</details>
<details>
    <summary>Hint 2</summary>
    <br>
    You can access the values of the array using <code>lw</code>.
</details>
<details>
    <summary>Hint 3</summary>
    <br>
    <code>lw</code> requires that the offset is an immediate value. When we compute the offset for this problem, it will be stored in a register. Since we cannot use a register as the offset, we can add the value stored in the register to the base address to compute the address of the index that we are interested in. Then we can perform a <code>lw</code> with an offset of <code>0</code>.
    <br>
    In the following example, the index is stored in <code>t0</code> and the pointer to the array is stored in <code>t1</code>. The size of each element is 4 bytes. In RISC-V, we have to do our own pointer arithmetic, so (1) we need to multiply the index by the size of the elements of the array. (2) Then we add this offset to the address of the array to get the address of the element that we wish to read and then (3) read the element.
    <br>
    <pre>
    <code>
    slli t2, t0, 2 # step 1 (see above)
    add t2, t2, t1  # step 2 (see above)
    lw t3, 0(t2) # step 3 (see above)
    </code>
    </pre>
    
</details>
