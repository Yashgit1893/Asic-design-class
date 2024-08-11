# Asic-design-class

<details>
<summary>LAB 1: C program to calculate sum from 1 to n (n=15) and then compiling the code with both GCC compiler</summary>

`sum1ton.c` is the file containing code to calculate the sum from 1 to n.

<p align="left">
  <img width="750" alt="1" src="https://github.com/user-attachments/assets/c03a8f66-e356-447a-815a-be940fdeec59">
</p>

Compiling the code using GCC compiler :
compiling the `sum1ton.c` with `gcc sum1ton.c` and run the executable file `./a.out`

<p align="left">
  <img width="750" alt="2" src="https://github.com/user-attachments/assets/9512912e-08f9-4a01-8950-b18ee442cfa4">
</p>

Output for sum from 1 to 15 is shown.

</details>

<details>
<summary>LAB 2: Compiling the sum1ton.c program of Lab 1 using RISC-V compiler. </summary>

Compiling the code using RISC-V compiler :

<p align="left">
  <img width="750" alt="3" src="https://github.com/user-attachments/assets/880561bb-45f1-466d-ada0-306014f6dbff">
</p>

compiling the `sum1ton.c` usung O1 optimization :
```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
```bash
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

<p align="left">
  <img width="750" alt="9" src="https://github.com/user-attachments/assets/ca1fdc9f-044e-4c64-b84a-ca40d518ba4f">
</p>

The number of instruction in the main function in 15

compiling the `sum1ton.c` using Ofast optimization :
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`
```
```bash
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

<p align="left">
  <img width="750" alt="6" src="https://github.com/user-attachments/assets/dac7aa97-b411-4c8e-b383-e54e9f5c3ebc">
</p>

Number of instruction in the main function is 12

Note : O1 optimization follows the standards but Ofast might break some programs, Ofast takes lesser number of instructions and provides better optimisation than O1

</details>



<details>
<summary>LAB 3: Compiling the sum from 1 to n(value of n in the code sum1ton.c of Lab 1 is changed to 100) C program using Spike simulator and debug the code. </summary>

compiling the `sum1ton.c` using the RISC-V compiler using the Ofast command :

```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

<p align="left">
 <img width="750" alt="5" src="https://github.com/user-attachments/assets/a67b6868-c6b6-4182-8bc4-45d7a476a575">
</p>

The above image shows the output using both `./a.out ` and `spike pk sum1ton.o`. Both of them have same output for sum from 1 to 100.

### Debug 

Debug the code using spike command :

```bash
spike -d pk sum1ton.o
```

command for spike debugger to run till instruction 100b0
```bash
until pc 0 100b0
```
to check the value at the register a2
```bash
reg 0 a2
```
<p align="left">
  <img width="750" alt="8" src="https://github.com/user-attachments/assets/4afaa25c-43f5-4e74-9997-76b20d08897c">
</p>

The above image displays how the value of a2 register changes while manual debugging.
The instruction `lui a2, 0x1` instruction changes the a2 register's value from 0x0000000000000000 to 0x0000000000001000.
The instruction `lui a0, 0x21` shows the a0 register value 0x0000000000021000.




Futher steps shows the vlaue at register sp. we again run the instructions from 0 to 100b8.

```bash
until pc 0 100b8
```

check the value at the register sp
```bash
reg 0 sp
```

The below image shows the manual debug.
`addi sp, sp, -16` instruction decrements the stack pointer (sp) by 16, The value of sp register was 0x0000003ffffffb50, and is now updated to 0x0000003ffffffb40

<p align="left">
  <img width="750" alt="7" src="https://github.com/user-attachments/assets/238392fa-484a-42fb-ae67-50418e61fef1">
</p>

Note :

1. lui stands for Load Upper Immediate, operates on the higher 16 bits and addi (Add Immediate) performs addition between the immediate value and the contents of the source register, then places the result into the destination register.

</details>


<details>
<summary>LAB 4: RISC-V Instructions</summary>
  
# RISC-V instructions


RISC-V (Reduced Instruction Set Computer - V) is an open standard instruction set architecture (ISA) based on established principles of RISC. The RISC-V ISA has a small number of instruction formats, making it relatively simple to understand and implement. The primary instruction formats in RISC-V are **R-type, I-type, S-type, B-type, U-type,** and **J-type**. Each format is designed for different types of operations and encodes different information in the instruction's 32 bits.


### 1. **R-type (Register)**
- **Explanation:**
  1. Used for arithmetic and logical operations involving two source registers and one destination register.
  2. Allows complex operations like addition, subtraction, and bitwise operations.
  3. Commonly used in ALU (Arithmetic Logic Unit) operations.

- **Instruction format:**
  - **opcode (7 bits):** Operation code that specifies the instruction.
  - **rd (5 bits):** Destination register.
  - **funct3 (3 bits):** Function modifier for opcode; helps determine the specific operation.
  - **rs1 (5 bits):** First source register.
  - **rs2 (5 bits):** Second source register.
  - **funct7 (7 bits):** Additional function modifier; often used to extend the opcode.

### 2. **I-type (Immediate)**
- **Explanation:**
  1. Handles operations with one source register and an immediate (constant) value.
  2. Used in load instructions, arithmetic with immediate values, and system calls.
  3. Simplifies operations that require a constant operand.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **rd (5 bits):** Destination register.
  - **funct3 (3 bits):** Function modifier.
  - **rs1 (5 bits):** Source register.
  - **imm[11:0] (12 bits):** Immediate value.

### 3. **S-type (Store)**
- **Explanation:**
  1. Facilitates storing data from a register into memory.
  2. Combines a base address (from a register) with an offset to calculate the memory address.
  3. Often used in conjunction with load instructions to manage memory.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **imm[11:5] (7 bits):** Upper bits of the immediate value.
  - **rs2 (5 bits):** Source register (data to store).
  - **rs1 (5 bits):** Base address register.
  - **funct3 (3 bits):** Function modifier.
  - **imm[4:0] (5 bits):** Lower bits of the immediate value.

### 4. **B-type (Branch)**
- **Explanation:**
  1. Used for conditional branching based on comparisons between two registers.
  2. Helps in implementing control flow constructs like loops and conditional statements.
  3. Modifies the program counter based on a branch condition.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **imm[12] (1 bit):** Immediate value (bit 12).
  - **imm[10:5] (6 bits):** Immediate value (bits 10-5).
  - **rs2 (5 bits):** Second source register.
  - **rs1 (5 bits):** First source register.
  - **funct3 (3 bits):** Function modifier.
  - **imm[4:1] (4 bits):** Immediate value (bits 4-1).
  - **imm[11] (1 bit):** Immediate value (bit 11).

### 5. **U-type (Upper Immediate)**
- **Explanation:**
  1. Used for loading a 20-bit immediate value into a register.
  2. Commonly used for creating large constants or setting up addresses.
  3. Essential for address calculation in conjunction with other instructions.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **rd (5 bits):** Destination register.
  - **imm[31:12] (20 bits):** Upper 20 bits of the immediate value.

### 6. **J-type (Jump)**
- **Explanation:**
  1. Directly modifies the program counter to jump to a new instruction address.
  2. Includes instructions like `jal` for jumping and linking return addresses.
  3. Useful for function calls and handling program control flow.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **rd (5 bits):** Destination register.
  - **imm[20] (1 bit):** Immediate value (bit 20).
  - **imm[10:1] (10 bits):** Immediate value (bits 10-1).
  - **imm[11] (1 bit):** Immediate value (bit 11).
  - **imm[19:12] (8 bits):** Immediate value (bits 19-12).

<p align="left">
  <img width="750" alt="1" src="https://github.com/user-attachments/assets/d5232af0-8c4e-4fe8-9aa9-7fc77967a66b">
</p>

### Explanation of the subfields :

### Immediate (`imm`)
A variable-length field embedded in instructions, representing a constant value. It's used directly as an operand or offset in various operations, such as arithmetic, memory access, and branching. The size and location differ across instruction formats, with some requiring sign extension.

### `funct7`
A 7-bit function code that specifies the operation variant within R-type instructions. It works with `opcode` and `funct3` to differentiate operations sharing the same basic function, like distinguishing between `add` and `sub`.

### `funct3`
A 3-bit field that further refines the operation specified by the `opcode`. It is used across multiple instruction formats (R-type, I-type, etc.) to indicate specific operations or conditions, such as arithmetic functions, memory operations, or branch types.

### `opcode`
A 7-bit field that identifies the general operation category (e.g., arithmetic, load/store, branch). It forms the basis for instruction decoding and works with `funct3` and `funct7` to determine the specific operation.

### Register Specifiers (`rs1`, `rs2`, `rd`)
- **`rs1` and `rs2`:** Source registers providing operands for an operation.
- **`rd`:** The destination register where the result is stored. These fields are central to operations across RISC-V formats, ensuring consistent and efficient register access.


### Instructions given

``` bash
 ADD r5, r6, r7
 SUB r7, r5, r6
 AND r6, r5, r7
 OR r8, r6, r5
 XOR r8, r5, r4
 SLT r10, r2, r4
 ADDI r12, r3, 5
 SW r3, r1, 4
 SRL r16, r11, r2
 BNE r0, r1, 20
 BEQ r0, r0, 15
 LW r13, r11, 2
 SLL r15, r11, r2
```
### Analyzing the instructions


### 1. **ADD r5, r6, r7** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r5` (`00001`)
   - **funct3:** `000` (for ADD)
   - **rs1:** `r6` (`00110`)
   - **rs2:** `r7` (`00111`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00111 00110 000 00001 0110011`

### 2. **SUB r7, r5, r6** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r7` (`00111`)
   - **funct3:** `000` (for SUB)
   - **rs1:** `r5` (`00101`)
   - **rs2:** `r6` (`00110`)
   - **funct7:** `0100000`
   - **Binary Encoding:** `0100000 00110 00101 000 00111 0110011`

### 3. **AND r6, r5, r7** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r6` (`00110`)
   - **funct3:** `111` (for AND)
   - **rs1:** `r5` (`00101`)
   - **rs2:** `r7` (`00111`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00111 00101 111 00110 0110011`

### 4. **OR r8, r6, r5** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r8` (`01000`)
   - **funct3:** `110` (for OR)
   - **rs1:** `r6` (`00110`)
   - **rs2:** `r5` (`00101`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00101 00110 110 01000 0110011`

### 5. **XOR r8, r5, r4** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r8` (`01000`)
   - **funct3:** `100` (for XOR)
   - **rs1:** `r5` (`00101`)
   - **rs2:** `r4` (`00100`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00100 00101 100 01000 0110011`

### 6. **SLT r10, r2, r4** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r10` (`01010`)
   - **funct3:** `010` (for SLT)
   - **rs1:** `r2` (`00010`)
   - **rs2:** `r4` (`00100`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00100 00010 010 01010 0110011`

### 7. **ADDI r12, r3, 5** (I-type)
   - **Opcode:** `0010011` (I-type)
   - **rd:** `r12` (`01100`)
   - **funct3:** `000` (for ADDI)
   - **rs1:** `r3` (`00011`)
   - **imm[11:0]:** `000000000101` (Immediate value = 5)
   - **Binary Encoding:** `000000000101 00011 000 01100 0010011`

### 8. **SW r3, r1, 4** (S-type)
   - **Opcode:** `0100011` (S-type)
   - **imm[11:5]:** `0000000` (High 7 bits of the immediate)
   - **rs2:** `r3` (`00011`)
   - **rs1:** `r1` (`00001`)
   - **funct3:** `010` (for SW)
   - **imm[4:0]:** `00100` (Low 5 bits of the immediate, 4 in decimal)
   - **Binary Encoding:** `0000000 00011 00001 010 00100 0100011`

### 9. **SRL r16, r11, r2** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r16` (`10000`)
   - **funct3:** `101` (for SRL)
   - **rs1:** `r11` (`01011`)
   - **rs2:** `r2` (`00010`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00010 01011 101 10000 0110011`

### 10. **BNE r0, r1, 20** (B-type)
   - **Opcode:** `1100011` (B-type)
   - **imm[12|10:5]:** `0000010` (High bits of the immediate, `00010`)
   - **rs2:** `r1` (`00001`)
   - **rs1:** `r0` (`00000`)
   - **funct3:** `001` (for BNE)
   - **imm[4:1|11]:** `00010` (Low bits of the immediate, `00100` for 20)
   - **Binary Encoding:** `0000010 00001 00000 001 00010 1100011`

### 11. **BEQ r0, r0, 15** (B-type)
   - **Opcode:** `1100011` (B-type)
   - **imm[12|10:5]:** `0000001` (High bits of the immediate, `00001`)
   - **rs2:** `r0` (`00000`)
   - **rs1:** `r0` (`00000`)
   - **funct3:** `000` (for BEQ)
   - **imm[4:1|11]:** `11110` (Low bits of the immediate, `01110` for 15)
   - **Binary Encoding:** `0000001 00000 00000 000 11110 1100011`

### 12. **LW r13, r11, 2** (I-type)
   - **Opcode:** `0000011` (I-type)
   - **rd:** `r13` (`01101`)
   - **funct3:** `010` (for LW)
   - **rs1:** `r11` (`01011`)
   - **imm[11:0]:** `000000000010` (Immediate value = 2)
   - **Binary Encoding:** `000000000010 01011 010 01101 0000011`

### 13. **SLL r15, r11, r2** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r15` (`01111`)
   - **funct3:** `001` (for SLL)
   - **rs1:** `r11` (`01011`)
   - **rs2:** `r2` (`00010`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00010 01011 001 01111 0110011`

</details>
