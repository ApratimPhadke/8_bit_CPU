# 8-Bit CPU From Scratch

A fully functional 8-bit CPU built from the ground up using logic gates in Digital Logic Sim by Sebastian Lague. This project demonstrates the complete design and implementation of a CPU architecture, from individual logic gates to a working processor.

## Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Components](#components)
- [Instruction Set](#instruction-set)
- [Building Process](#building-process)
- [How It Works](#how-it-works)
- [Gallery](#gallery)
- [Tools Used](#tools-used)

## Overview

This project is an 8-bit CPU designed and built entirely from fundamental logic gates. Every component, from simple AND/OR gates to complex ALU operations, was constructed manually to create a functioning processor capable of executing programs.

**Key Features:**
- 8-bit data bus architecture
- Custom instruction set
- 256 bytes of addressable memory (256x8 ROM)
- Full arithmetic and logic unit (ALU)
- Program counter with increment capability
- Multiple general-purpose registers
- Control unit for instruction decoding and execution

## Architecture

The CPU follows a simplified von Neumann architecture with the following main components:

### High-Level Block Diagram
```
┌─────────────┐
│   CPU BUS   │
└──────┬──────┘
       │
   ┌───┴────┐
   │  MAR   │ ← Memory Address Register
   └───┬────┘
       │
   ┌───┴────┐
   │  MEM   │ ← 256x8 ROM
   └───┬────┘
       │
   ┌───┴────┐
   │   IR   │ ← Instruction Register
   └───┬────┘
       │
   ┌───┴────────┐
   │ Control Unit│
   └─────┬──────┘
         │
    ┌────┴─────┐
    │   ALU    │ ← Arithmetic Logic Unit
    └──────────┘
```

### Component Overview

- **Program Counter (PC)**: Tracks the current instruction address, with increment capability
- **Memory Address Register (MAR)**: Holds the memory address for read/write operations
- **Memory (MEM)**: 256x8 bit ROM for program storage
- **Instruction Register (IR)**: Holds the current instruction being executed
- **Control Unit (CU)**: Decodes instructions and generates control signals
- **Registers (B, C, Tmp)**: General-purpose storage registers
- **Accumulator (Acc)**: Primary register for ALU operations
- **Arithmetic Logic Unit (ALU)**: Performs arithmetic and logical operations
- **Temporary Register (Tmp)**: Holds temporary values during operations

## Components

### 1. Memory System

#### 256x8 ROM
The memory is organized as 256 addresses, each storing 8 bits of data. It's built hierarchically:
- **8-bit memory cells**: Basic storage units built from D flip-flops
- **16x8 memory blocks**: Groups of 16 memory cells with addressing logic
- **256x8 ROM**: 16 blocks of 16x8 memory arranged in a grid

The memory uses a grid addressing scheme where the address is split into row and column selectors.

### 2. Program Counter (PC+1)

The program counter is an 8-bit counter that:
- Stores the address of the next instruction
- Automatically increments after each instruction fetch
- Can be loaded with a new value for jumps

Built using:
- 8 full adders chained together
- A program counter register to store the current value
- Increment logic that adds 1 on each clock cycle

### 3. Instruction Register

An 8-bit register built from D flip-flops that:
- Loads instructions from memory
- Holds the current instruction during execution
- Outputs to the control unit for decoding

### 4. Registers (B, C, Tmp)

General-purpose 8-bit registers for data storage:
- **B Register**: General-purpose storage, often used for ALU operations
- **C Register**: General-purpose storage
- **Tmp Register**: Temporary storage for intermediate values

Each register is built from 8 D flip-flops with enable and clock inputs.

### 5. Arithmetic Logic Unit (ALU)

The ALU is the heart of the CPU's computational capability. It performs:

#### Arithmetic Operations:
- **Addition**: 8-bit full adder chain with carry propagation
- **Subtraction**: Uses 2's complement addition

#### Logical Operations:
- **XOR**: Bitwise exclusive OR
- **AND**: Bitwise AND
- **OR**: Bitwise OR

The ALU is built from:
- 8 full adders for arithmetic operations
- XOR gates for each bit position
- Multiplexers to select between different operations
- Tri-state buffers for bus connection

### 6. Control Unit

The control unit orchestrates all CPU operations by:
- Decoding instructions from the instruction register
- Generating control signals for each component
- Managing the fetch-decode-execute cycle
- Handling timing and synchronization

Built using:
- Instruction decoder logic
- State machine for execution phases
- Control signal generators

### 7. Memory Address Register (MAR)

An 8-bit register that:
- Holds the memory address for the current operation
- Can be loaded from the bus or program counter
- Connects to the memory address lines

## Instruction Set

The CPU implements a custom instruction set (exact opcodes based on your architecture reference):

### Data Movement
- `LOAD`: Load data from memory to accumulator
- `STORE`: Store accumulator to memory
- `MOV`: Move data between registers

### Arithmetic Operations
- `ADD`: Add to accumulator
- `SUB`: Subtract from accumulator

### Logical Operations
- `AND`: Bitwise AND with accumulator
- `OR`: Bitwise OR with accumulator
- `XOR`: Bitwise XOR with accumulator

### Control Flow
- `JMP`: Unconditional jump
- `JZ`: Jump if zero flag is set
- `JC`: Jump if carry flag is set

## Building Process

### Phase 1: Basic Logic Gates
Started with fundamental gates:
- AND, OR, NOT gates
- NAND, NOR, XOR gates
- Built from simulated transistor-level logic

### Phase 2: Combinational Logic
Created more complex components:
- Multiplexers (2:1, 4:1, 8:1)
- Decoders (3:8, 4:16)
- Encoders
- Comparators

### Phase 3: Sequential Logic
Built memory and state elements:
- SR Latches
- D Flip-Flops
- Registers (8-bit)
- Counters

### Phase 4: Arithmetic Components
Developed computational units:
- Half Adder
- Full Adder
- 8-bit Ripple Carry Adder
- ALU with multiple operations

### Phase 5: Memory System
Created the memory hierarchy:
- 1-bit memory cell
- 8-bit memory word
- 16x8 memory block
- 256x8 ROM

### Phase 6: Control and Integration
Brought everything together:
- Control Unit design
- Bus architecture
- Instruction decoder
- Complete CPU integration

## How It Works

### Fetch-Decode-Execute Cycle

1. **Fetch Phase**:
   - Program counter value is loaded into MAR
   - Memory outputs instruction to IR
   - Program counter increments

2. **Decode Phase**:
   - Control unit reads instruction from IR
   - Generates appropriate control signals
   - Activates necessary data paths

3. **Execute Phase**:
   - Data is moved according to instruction
   - ALU performs required operation
   - Results are stored back to registers or memory

### Example Program Execution

```
Address | Instruction | Description
--------|-------------|-------------
0x00    | LOAD 0x10  | Load value from address 0x10 to accumulator
0x01    | ADD 0x11   | Add value at address 0x11 to accumulator
0x02    | STORE 0x12 | Store result to address 0x12
0x03    | JMP 0x00   | Jump back to start
```

## Gallery

### Full CPU Architecture

<img width="600" height="767" alt="image" src="https://github.com/user-attachments/assets/2564239d-3345-4e01-b984-966bcc63915b" />
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-39-14" src="https://github.com/user-attachments/assets/25ae7d93-e725-493d-aedc-5e6004c15a34" />
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-35-33" src="https://github.com/user-attachments/assets/9b528224-5813-4115-8159-4cf186c9ea4c" />


### Program Counter Detail

<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-36-04" src="https://github.com/user-attachments/assets/fd84787b-b0f6-4584-95ab-9dcbca84bb98" />
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-36-19" src="https://github.com/user-attachments/assets/04d2c594-5ae5-4188-a799-7c1afde8385e" />



### Instruction Register

<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-36-36" src="https://github.com/user-attachments/assets/e683e3ce-b310-4aed-b26f-6efcbcc11819" />


### Temporary Register
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-36-52" src="https://github.com/user-attachments/assets/483581f7-c3d3-4166-a4a3-ac146c982f48" />

### Control Unit
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-37-13" src="https://github.com/user-attachments/assets/7789c1ca-83e6-4ef5-a97c-d11e10969251" />


### ALU Architecture
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-37-29" src="https://github.com/user-attachments/assets/0a7d9b62-3481-434a-9e16-3c1b41e95cf2" />


### Memory System
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-37-49" src="https://github.com/user-attachments/assets/94f4b5e6-22b7-4a3a-ba9a-5f46862ade0c" />


### Memory Cell Detail
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-38-02" src="https://github.com/user-attachments/assets/f6458530-1019-472f-a894-7a7eef13288f" />


### Individual Memory Bit
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-38-18" src="https://github.com/user-attachments/assets/167970bb-cfbb-41e0-866e-6254054e203e" />


### Memory Address Register
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-38-35" src="https://github.com/user-attachments/assets/d1fc8a19-d8be-4448-b032-fa45a8ba2d46" />


### Accumulator
<img width="1440" height="900" alt="Screenshot from 2026-01-21 20-38-49" src="https://github.com/user-attachments/assets/2b60e3ab-d662-4d6d-8c5f-acc8bbc749fe" />


## Tools Used

**Digital Logic Sim** by Sebastian Lague
- A powerful digital logic simulator
- Allows gate-level circuit design
- Real-time simulation and testing
- Visual wire tracing for debugging

[Link to Digital Logic Sim](https://sebastian.itch.io/digital-logic-sim)

## Technical Specifications

- **Architecture**: 8-bit
- **Memory**: 256 bytes ROM
- **Registers**: 4 general-purpose (A, B, C, Tmp)
- **ALU Operations**: ADD, SUB, AND, OR, XOR
- **Address Bus**: 8-bit
- **Data Bus**: 8-bit
- **Clock**: Controlled simulation clock
- **Total Logic Gates**: ~10,000+ (estimated)

## Future Improvements

Potential enhancements for this project:
- [ ] Expand memory to include RAM
- [ ] Add more ALU operations (shift, rotate)
- [ ] Implement interrupts
- [ ] Add a stack pointer and stack operations
- [ ] Create an assembler for easier programming
- [ ] Build a simple I/O system
- [ ] Add conditional flags (zero, carry, negative)
- [ ] Implement a micro-coded control unit

## Learning Outcomes

This project provided hands-on experience with:
- Digital logic design from first principles
- Computer architecture fundamentals
- Sequential and combinational logic
- CPU design and organization
- Instruction set architecture
- Control unit design
- Memory systems and addressing
- Problem-solving and debugging complex systems

## Acknowledgments

- **Sebastian Lague** for creating Digital Logic Sim, an excellent tool for learning digital logic
- Various computer architecture textbooks and resources
- The classic 8-bit computer designs that inspired this project



---

**Built with patience, logic gates, and determination** 🔌⚡

*If you found this interesting or helpful, please consider giving it a star!* ⭐
