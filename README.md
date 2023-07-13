## Project Description

This project implements a 32-bit, single-cycle MIPS-like processor in Verilog. It is designed to execute a hardcoded bubble sort algorithm on a predefined array.

## File Description

* **`Core.v`:** Top-level module connecting all processor datapath components.
* **`Control Unit.v`:** Decodes instructions and generates the necessary control signals.
* **`ALU.v`:** Performs all arithmetic, logical, and comparison operations.
* **`memory.v`:** Contains separate instruction (`Text`) and data (`Data`) memories.
* **`mux.v`:** Provides multiplexer components for selecting data within the datapath.
* **`README.md`:** Documentation file explaining the processor's implementation and usage.
* **`tb.v`:** The main testbench used to simulate the entire core.
* **`alu_tb.v`:** A unit testbench for verifying the `ALU` module's functionality.
* **`mux_tb.v`:** A unit testbench for verifying the `mux` module's functionality.

## Structure

The modules work in concert to execute the bubble sort algorithm by repeatedly performing the fetch-decode-execute-memory-writeback cycle on instructions stored in memory.

1.  **Fetch:** The `Core` module's Program Counter (PC) provides an address to the `Text` memory, which fetches a single machine instruction (e.g., `lw $s4, 0($t0)` to load an array element `a[j]`).
2.  **Decode:** This instruction is sent to the `Control Unit`, which decodes it and generates specific control signals for the other components. For `lw`, it would enable reading from data memory and writing to a register.
3.  **Execute:** For instructions like `lw` or `add`, the `ALU` calculates a memory address by adding a register value (the array's base address) and an offset from the instruction. For a compare instruction like `ble` (`ble $s4, $s5, ...`), the `ALU` subtracts the two register values (`a[j]` and `a[j+1]`) to determine which is smaller.
4.  **Memory & Write-back:** The `Core` then accesses the `Data` memory. It either reads a value (for `lw`) to write back into the register file or writes a register's value into memory (for `sw` to perform the swap). This cycle repeats for every instruction in the bubble sort program, manipulating the array in `Data` memory until it is sorted.