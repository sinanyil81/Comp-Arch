# Digital Logic with Logisim
## Goals

- Get familiar with using the Logisim simulator
- Try to build advances logic circuits

## Introduction

This excercire series introduces [Logisim](http://www.cburch.com/logisim/) - an educational tool for designing and simulating digital logic circuits.

## Exercise 1: Get familiar with Logisim

You can run Logisim from inside the `Digital Logic` folder with
```console
java -jar tools/logisim-evolution.jar # If in a different folder, use the corresponding relative path 
```

After a short startup sequence, a Logisim window should appear. If not, check for errors in your terminal.

### Building a simple circuit

1. ![alt text](img/icon-and.png)  Start by clicking the `AND` gate button. This will cause the shadow of an `AND` gate to follow your cursor around. Click once within the main schematic window to place an `AND` gate.
2. ![alt text](img/icon-pin-input.png) Click the `Input Pin` button. Now, place two input pins somewhere to the left of your `AND` gate.
3. ![alt text](img/icon-pin-output.png) Click the `Output Pin` button. Then place an output pin somewhere to the right of your `AND` gate. Your schematic should look something like this at this point:

![alt text](img/circuit-and-disconnected.png)

4. ![alt text](img/icon-select.png) Click the `Select` tool button. Click and drag to connect the 2 input pins to the 2 inputs on the left side of the `AND` gate. You can only draw vertical and horizontal wires. Just draw a wire horizontally, release the mouse button, then click and drag starting from the end of the wire to continue vertically. Repeat the same procedure to connect the output on the right side of the `AND` gate to the output pin. After completing these steps your schematic should look similar to this:

![alt text](img/circuit-and-connected.png)

5. ![alt text](img/icon-poke.png) Finally, the `Poke` tool will toggle the values of the pins when you click on them. If you use the `Poke` tool on a wire, it will display the value on the wire. Select the `Poke` tool, try clicking on the input pins in your schematic, and observe what happens. Does the output match with what you think an `AND` gate should do? Now, try poking a wire directly. The current value on that wire should pop up; this is very useful for more complex circuits.
6. Now, delete the wires, and try wiring each input pin to the other pin on the `AND` gate, in such a way that the wires cross over. An extreme example:
![alt text](img/circuit-and-crossover.png)
If you're creating a wire and drag it **over** another wire without stopping, the wires will not connect. If you're creating a wire and stop dragging while **on top** of another wire, a junction (big circle) will be created, and the wires will connect. Make sure to pay attention to junctions when you're designing your circuits!

### List of Wire Colors and Values

Please take a look at this list. It may help to try re-creating each color on your own. 
![alt text](img/circuit-wire-errors.png)
| Color       | Meaning |
| ----------- | ----------- |
| Dark green      | 1-bit wire has a value of 0       |
| Bright green  | 1-bit wire has a value of 1       |
|Black|Multi-bit wire (many components have bit width attributes which can be configured in the attributes menu on the bottom left)|
|Red (values with `EEEE`)|The wire has multiple values on it (in this case, a `0` and `1` from the two inputs). Also, remember that a big circle appears at wire junctions.|
|Blue (values with `UUUU`)|The wire is floating (i.e. has no known value)|
|Orange|The wire is connected to components that have different bit widths. A simple example is a 1-bit input pin connected to a 2-bit output pin. In the shown example, there is a slightly hidden wire behind the `MUX` connecting the 2-bit lower data line to the 1-bit select line. Watch out for these!|

## Exercise 2: Sub-Circuits

Just as C programs can contain helper functions, a schematic can contain helper subcircuits. In this part of the lab, we will create several subcircuits to demonstrate their use.

**Note:** Logisim generally doesn't permit names with spaces or symbols, names starting with numbers, or names that conflict with keywords (e.g., `NAND`).

### Action

Follow the steps below. Remember to **save often**, and **avoid moving or editing the provided input/output pins**.

1. Open up the Exercise 2 schematic (`File -> Open -> ex2/ex2.circ`).
2. Open up the `AND2` sample subcircuit by double-clicking `AND2` in the circuit selector on the left side. 
![alt text](img/interface-circuit-selector.png)
Note the `2` at the end; because there is a component called `AND`, we cannot call it `AND`. We've created a demo circuit for your reference. It has 2 1-bit input pins, `A` and `B`, and sends the result of `A & B` to the `RESULT` output pin. This should look very similar to the practice circuit you just made.
3. Now, open up the `NAND2` subcircuit. It's time to make your own circuit! Fill in this circuit **without** using the built-in `NAND` gate from the Gates library on the left (i.e., only use the `AND`, `OR`, and `NOT` gates; they are available as little icons in the toolbar at the top of the window, or in the Gates library in the circuit selector). When you are done, similarly fill in the `NOR2`, `XOR2`, `MUX2` (2-to-1 MUX), and `MUX4` (4-to-1 MUX). Please note that `NAND`, `NOR`, `XOR`, and `MUX` already exist in Logisim. This exercise is meant to help you understand how to use subcircuits. 

Important notes:
- Please do not change the names of the subcircuits or create new ones, otherwise your circuit may not work properly
- Don't use any built-in gates other than `AND`, `OR`, and `NOT`. However, once you have built a subcircuit, you may (and are encouraged to) use it to build others. You can do this by single-clicking a subcircuit in the circuit selector and placing it like you did for the `AND/NOT/OR` gates
- It is helpful to write out a truth table for each circuit.
- For the 4-to-1 `MUX`, `SEL0` and `SEL1` correspond to the 0th and 1st bits of the 2-bit selector, respectively. Make sure not to switch them!

### Testing

Open a terminal session and go to the `Digital-Logic` folder. There are tests provided for each exercise, which you can run with 
```console
python3 test.py
```
For each test, your circuit is run in a test harness circuit (`tests/ex2-test.circ`), and its output is compared against the reference output for that test (`tests/reference-output/ex2-test.out`). In the output file, each column corresponds to an input/output pin on the main circuit, and each row shows a set of inputs and the corresponding outputs the circuit produced. If your circuit output (`tests/student-output/ex2-test.out`) is different, you can check it against the reference output file; the `diff` command may help.

- Note that the output files are "formatted" by adding tab characters (`\t`) between each value, and they look best when tabs are displayed as 8 spaces wide. Values and column headers that are 8+ characters might mess up the alignment, so watch out for those!
- Don't modify the reference output files, otherwise local tests might produce incorrect results
- You shouldn't need to edit the test harness circuits for this lab. However, it might be useful to take a look!