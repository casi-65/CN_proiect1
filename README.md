# 8-bit Arithmetic Logic Unit (ALU)

## Project Overview

This project implements an 8-bit Arithmetic Logic Unit (ALU) capable of performing basic arithmetic operations: addition, subtraction, multiplication, and division on two 8-bit operands. The design is modular and consists of two main components:

- Arithmetic Unit
- Control Unit

The implementation is written in Verilog and tested using ModelSim.

## Architecture

### Arithmetic Unit

This module performs operations on the input data when triggered by control signals from the control unit.

**Submodules:**
- `reg_8` – 8-bit register for operand M
- `shift_reg_16` – 16-bit register and shifter for A and Q
- `extraQ` – 1-bit register storing Q[-1]
- `adder` – 8-bit adder/subtractor based on the operation signal
- `counter` – 3-bit incrementing counter

### Control Unit

This is a finite state machine (FSM) responsible for controlling the data path and coordinating the execution flow based on the selected operation.

**Submodules:**
- `reg4bit` – stores the current FSM state
- `add_sum_fsm` – FSM for addition and subtraction
- `mul_fsm` – FSM for multiplication (Booth Radix-2)
- `div_fsm` – FSM for division (Restoring Division)

## Control Signals

The ALU relies on a set of control signals to manage data flow between registers and modules:

| Signal | Description |
|--------|-------------|
| `C0`   | Load INBUS into M |
| `C1`   | Load INBUS into Q |
| `C2`   | Load adder result into A |
| `C3`   | Select adder operation (0 = add, 1 = subtract) |
| `C4`   | Trigger shift operation on {A, Q} (one per clock cycle) |
| `C5`   | Increment counter |
| `C6`   | Select bit value used in shifting |
| `C7`   | Load A into OUTBUS |
| `C8`   | Load Q into OUTBUS |
| `C9`   | Load INBUS into A |
| `C10`  | Load the value of C6 into Q[0] |

## Complex Operations

The ALU supports two optimized algorithms for complex arithmetic:

- **Booth Radix-2 Multiplication**: implemented using an FSM to reduce hardware complexity.
- **Restoring Division**: used for binary division through iterative subtraction and shifting.

These methods were chosen for their simplicity and efficiency in hardware-based implementations.

## Testing

Testing was conducted using:

- **ModelSim** – for simulation and waveform analysis
- **Digital** – for building and validating submodules
- **Draw.io** – for architecture and FSM diagrams

## File Structure

- `arithmetic_unit.v` – arithmetic logic implementation
- `control_unit.v` – FSM logic and control signal generation
- `alu_top.v` – top-level module connecting all components
- `testbench.v` – simulation and verification environment
- `README.md` – project documentation
