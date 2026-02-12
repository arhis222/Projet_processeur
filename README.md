# Toy Processor Enhancement (ArM)

This project focuses on the modification and enhancement of a synchronous "toy" processor designed using the **Lustre** programming language. The objective was to extend the processor’s instruction set and improve its control logic, particularly for conditional execution and branching mechanisms.

---

## Architecture Overview

The processor follows a classical **Hardware/Software Co-design** approach, structured into two main parts:

### Operative Part (PO)
The data path handles the storage and manipulation of data. It includes:
* **Registers**: Accumulator (`ACC`), Program Counter (`PC`), and Instruction Registers (`RI1`, `RI2`).
* **ALU (UAL)**: Responsible for arithmetic and logic operations.
* **Status Flags**: Registers for `Z`, `N`, `V`, and `C` to store the results of calculations.
* **Multiplexers**: Used for operand selection and address routing (`SelBusOp`, `SelBusAd`).



### Control Part (PC)
**Finite State Machine (FSM)** responsible for managing the processor's lifecycle:
* **Sequencing**: Controlling the instruction fetch, decode, and execute cycles.
* **Signal Generation**: Producing command signals for the PO based on decoded instructions and status flags.

---

## Key Enhancements

### SUB Instruction
Added support for the subtraction instruction:
* **Command**: `SUB [Address]`
* **Operation**: `ACC = ACC - Mem[Address]`
* **Implementation**: Modified the `procPC.lus` automaton and `procPO.lus` data path to utilize the ALU's subtraction capability.

### Flag Memorization
Four flip-flops were integrated to store status flags resulting from `ADD` or `SUB` operations:
* **Flags**: `Z` (Zero), `N` (Negative), `V` (Overflow), and `C` (Carry).
* **Logic**: Flags are updated only during calculation instructions and remain stable during other cycles, such as PC increments.

### Conditional Branching – BHI
Introduced a new conditional branch instruction:
* **Command**: `BHI label` (*Branch if Higher*)
* **Condition**: Branches if `C AND NOT Z` is true.
* **Logic**: This enables strictly greater-than comparisons in unsigned binary arithmetic.

---

## Automaton & Control Logic

The Control Part utilizes an **11-state FSM** (States 0 through 10) to manage sequencing:
* **State 0**: Initialization and PC reset.
* **States I–II**: Instruction fetch from memory and decoding into registers.
* **States III–VIII**: Execution of specific operations (LOAD, STORE, ADD, SUB, JUMP, and BHI).



---

## Testing & Validation

Validation was performed using the **Luciole simulator**The following test program was used to verify arithmetic and branching:

1. `LOAD #3` → Set Accumulator to 3.
2. `STORE [8]` → Save value to memory address 8.
3. `LOAD #5` → Load 5 into Accumulator.
4. `Etiq2: SUB [8]` → Subtract 3 from current Accumulator.
5. `BHI Etiq2` → Loop if result is strictly greater than 0.

Simulation results confirmed correct flag capture (e.g., `FlagC` activation) and accurate branching behavior.

---

## Project Structure

* `procPO.lus`: Implementation of the Operative Part (Registers, Mux, Flags).
* `procPC.lus`: Implementation of the Control Part (FSM and Automaton logic).
* `UALproc.lus`: ALU component defining arithmetic operations).

---

## Author
* **Arhan UNAY**

---

### Academic Context

Developed as part of the **INFO3 – Architecture Matérielle (ArM)** course  
Polytech Grenoble – April 2025

