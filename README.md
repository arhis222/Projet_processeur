# Toy Processor Enhancement (ArM)

This project focuses on the modification and enhancement of a synchronous "toy" processor designed in the **Lustre** programming language. The goal was to extend the processor's instruction set and improve its control logic for conditional operations.

## 📌 Project Overview
The enhancement involves adding arithmetic capabilities and sophisticated branching logic to a base processor architecture, following the standard **Control Part (PC)** and **Operative Part (PO)** separation.

### 🛠️ Key Enhancements
* **SUB Instruction Implementation:** Added the `SUB [Address]` instruction, enabling the accumulator (`ACC`) to perform subtraction: `ACC = ACC - Mem[Address]`.
* **Flag Memorization:** Integrated four D-type flip-flops to capture and store status flags (`Z`, `N`, `V`, `C`) during calculation instructions (`ADD` or `SUB`). Crucially, these flags remain stable during non-calculation cycles (like PC increments).
* **Conditional Branching (BHI):** Introduced the `BHI label` (Branch if Higher) instruction. The processor performs a jump if the logical condition `C AND NOT Z` is met, allowing for "strictly greater than" comparisons.

## ⚙️ Architecture & Design
* **Control Part (PC):** The FSM (Finite State Machine) in `procPC.lus` was modified to decode the new instruction opcodes and manage the synchronization of flag storage.
* **Operative Part (PO):** The data path in `procPO.lus` was updated to route status flags from the ALU back to the controller and handle the subtraction data flow.
* **ALU:** Utilized the existing subtraction operations within the `UALproc.lus` component.

## 🚀 Testing & Validation
The design was verified using the **Luciole** simulator with a test program that executes a loop:
1.  Initialize values in memory.
2.  Perform repeated subtractions.
3.  Use `BHI` to loop until the accumulator value is no longer strictly greater than the operand.
4.  Verify final results in the Accumulator and Memory.

## 📂 File Structure
* `procPC.lus`: Modified controller automaton.
* `procPO.lus`: Updated operative part with flag registers.
* `UALproc.lus`: ALU logic for arithmetic operations.
* `top.lus`: Top-level integration of the processor.

## 👥 Authors
* **Utku GEMICIOGLU**
* **Arhan UNAY**

---
*Developed as part of the INFO3 - Architecture Matérielle (ArM) course at Polytech Grenoble, April 2025.*
