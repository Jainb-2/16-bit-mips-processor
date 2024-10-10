# 16-bit MIPS Processor Verification using UVM

## Project Overview
This project focuses on verifying a 16-bit five-stage pipelined MIPS processor with 16 instructions and 49 variants using the **Universal Verification Methodology (UVM)** in SystemVerilog. The design includes essential concepts like **pipelining**, **data dependencies**, and **hazard unit management**. The verification leverages both **Constrained Random Verification (CRV)** and **Assertion Based Verification (ABV)** to test the functionality of the MIPS processor.

## Table of Contents
1. [Design Specifications](#design-specifications)
2. [Design Under Test](#design-under-test)
3. [Instruction Set](#instruction-set)
4. [Inputs and Outputs](#inputs-and-outputs)
5. [Verification Plan](#verification-plan)
6. [Test Coverage](#test-coverage)
7. [Layered Testbench Architecture](#layered-testbench-architecture)
8. [Conclusion](#conclusion)

---

## Design Specifications
- **Processor Type**: 16-bit, 5-stage pipelined MIPS processor.
- **Instructions**: 16 instructions with 49 variants.
- **Stages**: Instruction Fetch, Decode, Execute, Memory Access, Write Back.
- **Verification Methodology**: UVM (Universal Verification Methodology).
- **Verification Techniques**: 
  - Constrained Random Verification (CRV)
  - Assertion Based Verification (ABV)

---

## Design Under Test
The MIPS processor consists of the following stages:
1. **Instruction Fetch**: Fetches instructions to be executed.
2. **Decode**: Decodes the fetched instructions, controlling registers and control signals.
3. **Execution**: Performs arithmetic and logical operations.
4. **Memory Access**: Accesses data from memory.
5. **Write Back**: Writes the processed data back into the appropriate registers.

---

## Instruction Set
The instruction set is divided into functional categories:

| Instruction   | Operation          | Opcode | Example |
|---------------|--------------------|--------|---------|
| **Addition**  | C = A + B          | 0000   | z = x + y |
| **Subtraction**| C = A - B         | 0001   | z = x - y |
| **Increment** | C = A + 1          | 0011   | z = x + 1 |
| **Decrement** | C = A - 1          | 0010   | z = x - 1 |
| **AND**       | C = A & B          | 0100   | z = x & y |
| **OR**        | C = A | B          | 0101   | z = x | y |
| **XOR**       | C = A ^ B          | 0110   | z = x ^ y |
| **Shift Left**| C = A << 1         | 1100   | -       |
| **Shift Right**| C = A >> 1        | 1101   | -       |
| **Load**      | C = mem[address]   | 1010   | -       |
| **Store**     | mem[address] = A   | 1011   | -       |
| **Move**      | C = A              | 1001   | -       |
| **Jump**      | PC = new address   | 1101   | -       |
| **NOP**       | No Operation       | 1110   | -       |
| **EOP**       | End of Process     | 1111   | -       |

---

## Inputs and Outputs

| Signal Name     | Direction | Width  | Description                                           |
|-----------------|-----------|--------|-------------------------------------------------------|
| `inst_in`       | Input     | 16 bits| Instruction input to the pipeline.                    |
| `reg_write`     | Output    | 16 bits| Data to be written back to registers.                 |
| `mem_write`     | Output    | 16 bits| Data to be written into memory.                       |
| `jump_enable`   | Output    | 1 bit  | Signal to indicate jump condition.                    |
| `rd_mem_addr`   | Output    | 3 bits | Address from which data is read in memory.            |

---

## Verification Plan

The verification plan is divided into two primary parts:

### 1. **Data-Independent Instruction Testing**
- **Goal**: Verify each instruction's functionality independently.
- **Method**: Constrained Random Verification (CRV) generates random instruction sequences, and the outputs are checked against expected results.

### 2. **Data-Dependent Instruction Testing**
- **Goal**: Verify data dependencies between instructions (e.g., load, store, move).
- **Method**: Assertion Based Verification (ABV) checks that data dependencies are correctly resolved by the hazard unit and control unit.

---

## Test Coverage

### CRV Test Cases:
- **Addition**: Verify correct output for `C = A + B`.
- **Subtraction**: Check `C = A - B`.
- **AND/OR/XOR**: Validate logical operations between registers.
- **Shift Left/Right**: Test left and right shift operations.

### ABV Test Cases:
- **MOV/LOAD/STORE**: Verify that the data is correctly transferred between memory and registers with dependencies handled.
- **NOP/EOP**: Ensure the processor correctly handles NOP (no operation) and EOP (end of process).

### Coverage Bins:
Each opcode is assigned specific coverage bins to track the verification progress and ensure that all configurations are tested.

---

## Layered Testbench Architecture

The testbench is structured using the UVM framework:

1. **Processor Testbench Package**:
   - A central package file to consolidate all test modules for easy execution.

2. **Processor Test**:
   - Extends from `uvm_test`, initiating test sequences for the MIPS processor.

3. **Processor Top**:
   - Instantiates the DUT (Design Under Test) and connects the testbench.

4. **Processor Environment**:
   - Creates the test environment with agents, monitors, and the scoreboard.

5. **Processor Agent**:
   - Manages the communication between the DUT and the testbench.

6. **Processor Monitor**:
   - Captures DUT input/output for verification.

7. **Processor Driver**:
   - Drives inputs to the DUT during testing.

8. **Processor Scoreboard**:
   - Compares DUT outputs against expected values for result validation.

9. **Processor Subscriber**:
   - Records coverage and generates reports for overall test coverage.

---

## Conclusion

This project successfully verified the functionality of a five-stage pipelined 16-bit MIPS processor using UVM. The layered testbench ensured systematic verification, covering a wide range of instructions and dependencies. The use of CRV and ABV enhanced the verification process, providing a strong foundation for further MIPS processor exploration and testing.

---

## Future Work

- Expand instruction set coverage to include additional variants.
- Enhance the coverage model to include timing analysis for pipeline hazards.
- Explore power and performance analysis under different operating conditions.


