# RISC-V Based MYTH Workshop

## Workshop Contents

- [Day 1 - Introduction to RISC-V ISA and GNU Compiler Toolchain](#day-1---introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
- [Day 2 - Introduction to ABI and Basic Verification Flow](#day-2---introduction-to-abi-and-basic-verification-flow)
- [Day 3 - Digital Logic with TL-Verilog and Makerchip](#day-3---digital-logic-with-tl-verilog-and-makerchip)
- [Day 4 - Basic RISC-V CPU Micro-architecture](#day-4---basic-risc-v-cpu-micro-architecture)
- [Day 5 - Complete Pipelined RISC-V CPU Micro-architecture](#day-5---complete-pipelined-risc-v-cpu-micro-architecture)

## Day 1 - Introduction to RISC-V ISA and GNU Compiler Toolchain

<details>
  <summary><b>RV Day 1</b></summary>
  
### D1SK1 - Introduction to RISC-V basic keywords  
**Instruction Set Architecture(ISA):**  
An Instruction Set Architecture (ISA) is the interface between a computer's hardware and software. It defines the set of instructions a processor can execute, including operations like arithmetic, data movement, and control flow. Example : x86, ARM , RISC-V

**The Bigger Picture:**
```mermaid
graph TD;
  subgraph B["System Software"]
    D["OS"]-->E["Compiler"];
    E["Compiler"]-->F["Assembler"];
  end
  A["Application Software"]-->B;
  B-->C[Hardware];
```
An application software that we use on PCs or mobiles are converted into binary language(machine code) by the system software which is then executed by the hardware.
System Software
-	**Operating System(OS)**
    -  Handle I/O operations, allocate memory, low level system functions
-	**Compiler**
    - Converts the application written in high level language(C, C++, Java) into assembly language of the respective ISA.
-	**Assembler**
    - The Assembly code from previous step is converted to binary language by the Assembler.
 
**Contents :** 
-	Pseudo Instructions :
  
  <p align="center">
    <img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/7b34011e-7ffe-40d5-8bcf-2b98af58d64f" />
  </p>

-	Base integer Instructions RV64I :

  <p align="center"> 
    <img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/ee5d4662-9aef-4239-b18e-8660cebf40db" />
  </p>

-	Multiply extension RV64M :
  
  <p align="center"> 
    <img width="1918" height="1074" alt="image" src="https://github.com/user-attachments/assets/0d19a352-2d1c-479d-a2d3-ae3a1ce30602" />
  </p>
  
-	Single and duouble precision floating point extension RV64F & RV64D :
  
  <p align="center"> 
    <img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/a21c647a-3343-44c4-a290-83d2ae617f2d" />
  </p>
  
-	Application Binary Interface :
  
  <p align="center"> 
    <img width="1916" height="1079" alt="image" src="https://github.com/user-attachments/assets/f4d26bf8-7476-4d71-9c17-c8848d68e6b5" />
  </p>
  
-	Memory allocation and stack pointer :
  
  <p align="center"> 
    <img width="1917" height="1076" alt="image" src="https://github.com/user-attachments/assets/f464851b-002e-4c00-940d-aaf6f09bebf2" />
  </p>

### D1SK2 - Labwork for RISC-V software toolchain    

```bash
# cd into the root directory
cd 

# open a file using an editor to write the C code to compute the sum of n natural numbers
gvim sum1ton.c

# Compile the code using the gcc compiler
gcc sum1ton.c

# Run the binary output file 
./a.out
```

  <p align="center"> 
    <img width="1289" height="803" alt="image" src="https://github.com/user-attachments/assets/b978ae1f-7c66-461b-a304-f722ef0d5584" />
  </p>
  <p align="center"> 
    <img width="1290" height="806" alt="image" src="https://github.com/user-attachments/assets/110df575-8b3b-4fb7-b303-331ab82c7dbb" />
  </p>  
  
```bash

# Compile the code using the RISC-V gcc compiler
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c

# check for the output file 
ls -ltr ./sum1ton.o

# To view the object and disassembler file
riscv64-unknown-elf-objdump -d sum1ton.o >> sum1ton.dis
gvim sum1ton.dis
```

  <p align="center">
    <img width="1300" height="271" alt="image" src="https://github.com/user-attachments/assets/fdf0cbb4-636a-4ea1-b17a-e338c7e01eb8" />
  </p>
  <p align="center">
    <img width="1288" height="800" alt="image" src="https://github.com/user-attachments/assets/d21d894f-c075-4b40-b6ff-0ca2c29d9b05" />
  </p>  

```math
Number\ of\ instructions = \frac{(End\ address\ +\ 4\ -\ Start\ address)}{4}  
```
```math
Start\ address\ of\ main\ function\ =\ 10184  
```
```math
End\ address\ of\ main\ function\ = 101bc  
```
```math
Number\ of\ instructions = \frac{(101bc\ +\ 4\ -\ 10184)}{4}  = 0xF = 15
```

```bash

# Compile the code using the RISC-V gcc compiler
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c

# To view the object and disassembler file
riscv64-unknown-elf-objdump -d sum1ton.o >> sum1ton.dis
gvim sum1ton.dis
```

  <p align="center">
    <img width="1287" height="808" alt="image" src="https://github.com/user-attachments/assets/7ba159a7-c5ae-4f8a-88bd-bc06c290df81" />
  </p>

```math
Start\ address\ of\ main\ function\ =\ 100b0  
```
```math
End\ address\ of\ main\ function\ = 100dc  
```
```math
Number\ of\ instructions = \frac{(100dc\ +\ 4\ -\ 100b0)}{4}  = 0xC = 12
```


```bash

# Compile the code using the RISC-V gcc compiler
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c

# To execute the output of previous step
spike pk sum1ton.o
```

  <p align="center">
    <img width="1284" height="288" alt="image" src="https://github.com/user-attachments/assets/f59766ac-eb91-4025-8d4e-b5b5b45a210f" />
  </p>

```bash

# To run in debug mode
spike -d pk sum1ton.o

# To run until a fixed address
until pc 0 100b0

# Pressing Enter key executes the next instruction

# To view the value in a register
reg 0 a2
```

<p align="center">
  <img width="1285" height="804" alt="image" src="https://github.com/user-attachments/assets/33802f88-66e6-4cc2-9b24-d49d88e1bc7b" />
</p>

### D1SK3 - Integer Number Representation    

Computers store and process data in the form of voltages. There are 2 digits in this system, when the voltage level is above a threshold it is represented by 1, when it is below a threshold it is considered 0. Each symbol in this system is called a bit (binary digit).  
8 bits  - 1 byte  
32 bits - 1 Word  
64 bits - 1 Double Word  

<p align="center">
  <img width="1917" height="1073" alt="image" src="https://github.com/user-attachments/assets/15ad40a4-f255-46b9-aa0b-6cbe9cfa6d47" />
</p>

The number of symbols that can be represented using n-bits is `2ⁿ`. So the range for unsigned integer is `0` to `2ⁿ-1`. For a signed integer the range is `−2ⁿ⁻¹` to `2ⁿ⁻¹ − 1`.
  


<p align="center">
  <img width="1753" height="745" alt="image" src="https://github.com/user-attachments/assets/a25ba43b-a0db-46ae-a34d-c987994bd929" />
</p>
<p align="center">
  <img width="1697" height="362" alt="image" src="https://github.com/user-attachments/assets/8e18a361-28c7-4a60-835e-3d6428ec2e71" />
</p>

The negative numbers are represented using 2's complement.

<p align="center">
  <img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/a73d3d31-01e2-4b40-a4c7-76d1cc6c3014" />
</p>
<p align="center">
  <img width="1915" height="1079" alt="image" src="https://github.com/user-attachments/assets/867acf68-e4f0-4672-8859-cbc7d58c76f3" />
</p>
<p align="center">
  <img width="1918" height="569" alt="image" src="https://github.com/user-attachments/assets/1290fe6e-5990-4417-9aef-1bf2a58f839f" />
</p>
Instructions that operate on these kind of numbers are called Base integer instructions RV64I.  

### Lab

<p align="center">
  <img width="1289" height="802" alt="image" src="https://github.com/user-attachments/assets/2fddef9d-c06b-4ba4-ba62-074c2be76d8d" />
  <img width="1284" height="479" alt="image" src="https://github.com/user-attachments/assets/d270f6f1-c676-4f19-9c96-b2d36adcecdb" />
  <img width="1288" height="487" alt="image" src="https://github.com/user-attachments/assets/c24e634d-5594-4376-b7a6-a34863358a5b" />
  <img width="1285" height="492" alt="image" src="https://github.com/user-attachments/assets/92e6c021-dbef-413e-91cd-82f90fb34337" />
  <img width="1289" height="490" alt="image" src="https://github.com/user-attachments/assets/2daa0946-b819-4865-9440-794387a1e7ac" />
  <img width="1285" height="513" alt="image" src="https://github.com/user-attachments/assets/0e7b84f8-ae80-4b74-986d-49523b261534" />
  
  The reason the actual value is not shown is because of the typecasting.
  <img width="1285" height="600" alt="image" src="https://github.com/user-attachments/assets/f9cfd957-db00-4f3a-92d6-656457cafe8a" />
  <img width="1459" height="466" alt="image" src="https://github.com/user-attachments/assets/84c4e5bd-a765-4c55-9454-89bcff255088" />
</p>

</details>
    
## Day 2 - Introduction to ABI and Basic Verification Flow

<details>
<summary><b>RV Day 2</b></summary>
  
### D2SK1 Application Binary Interface (ABI)  
<details>
<summary><b>Theory Images</b></summary>
  <p align="center">
    <img width="1778" height="883" alt="image" src="https://github.com/user-attachments/assets/12bce583-0e54-4f17-ae25-082dd2bdf0ea" />
    <img width="1773" height="1029" alt="image" src="https://github.com/user-attachments/assets/109c1e21-b497-4dde-aef1-ad5a4645fd01" />
    <img width="1572" height="202" alt="image" src="https://github.com/user-attachments/assets/053ebe1b-0580-4d2e-a712-4271d4e79c6a" />
  </p>  
</details>  

There are 32 Registers in RISC-V architecture and XLEN defines the size of the registers, it can be 32 for RV32 and 64 for RV64.
<p align="center">
  <img width="1582" height="1066" alt="image" src="https://github.com/user-attachments/assets/4f3a58f6-6224-4ba9-b9b2-6131f66a708d" />
</p>

The data can be loaded on to the registers in 2 ways.  
- It can be loaded directly.
- It can be loaded from a location in the memory.

  The memory is byte addressable and based on how the bytes are arranged in the memory, it is classified as Big Endian and Little Endian.

**Endianness** defines the order in which bytes of a multi-byte data value are stored in memory.

- **Big Endian**: Most Significant Byte (MSB) is stored at the lowest memory address.
- **Little Endian**: Least Significant Byte (LSB) is stored at the lowest memory address.

#### Example

Consider the 32-bit value: 0x12345678  


| Memory Address | Big Endian | Little Endian |
|---------------|------------|---------------|
| 0x00 | 12 | 78 |
| 0x01 | 34 | 56 |
| 0x02 | 56 | 34 |
| 0x03 | 78 | 12 |

Although the **byte order in memory differs**, both representations store the same value `0x12345678`.

> Most modern processors, including **RISC-V**, use **little-endian** byte ordering by default.

<details>
<summary><b>Endian Images</b></summary>
  <p align="center">
    <img width="1918" height="1074" alt="image" src="https://github.com/user-attachments/assets/9dda5d49-8835-4603-9cc7-6d0100fa3bb5" />
    <img width="1918" height="1076" alt="image" src="https://github.com/user-attachments/assets/fa49245d-33da-48d3-acf8-2678f9b109a3" />
    <img width="1917" height="1075" alt="image" src="https://github.com/user-attachments/assets/cbbd2f10-98d0-4f6d-b385-0a0dbaa07830" />
  </p>  
</details>  

For example, let us consider an array M with 3 doublewords and we want to load the data in 3rd doubleword to the register x8. The instruction ld loads the 64 bit data at the address obtained by adding offset (16) to the contents of the source register rs1 (x23, in this case x23 has 0) to the destination register rd (x8).  
  <p align="center">
    <img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/703cfea2-a694-4a42-8e29-4c6cfd7b995d" />
  </p>

All the instructions in RISC-V are 32 bits (irrespective of RV64 or RV32).   
<p align="center">
  <img width="1485" height="214" alt="image" src="https://github.com/user-attachments/assets/73629cfc-7f52-4b50-9a04-4127ee20517b" />
  <img width="1458" height="633" alt="image" src="https://github.com/user-attachments/assets/1e7f8449-c2c0-4d24-96c9-b77beafee9e6" />
  <img width="1539" height="740" alt="image" src="https://github.com/user-attachments/assets/3e7d1989-1f65-4e46-8e0b-bef2e34d52dd" />
  <img width="1621" height="748" alt="image" src="https://github.com/user-attachments/assets/b6d2da25-742f-4c66-95a0-916f8ffc3f2c" />
  <img width="1743" height="1056" alt="image" src="https://github.com/user-attachments/assets/3c0da88e-6b2a-4c51-a7ef-e2ff3a69bfce" />
</p>  



### D2SK2 Lab work using ABI function calls  
<p align="center">
  <img width="909" height="591" alt="image" src="https://github.com/user-attachments/assets/5f46efcf-2caa-4ae8-af8d-e0477826c17d" />
  <img width="1344" height="1043" alt="image" src="https://github.com/user-attachments/assets/f9aa1994-58f9-4173-8176-09f6498ea33f" />
  <img width="1285" height="811" alt="image" src="https://github.com/user-attachments/assets/ee753269-d382-4220-bfaa-43d9641792fb" />
  <img width="1286" height="802" alt="image" src="https://github.com/user-attachments/assets/49fa1143-7921-4c73-bb98-41fdb50b8f00" />
  <img width="1286" height="805" alt="image" src="https://github.com/user-attachments/assets/676ffea9-81ff-47f4-ad5e-0bfd6c5e0d09" />
</p>

### D2SK3 Basic verification flow using iverilog
<p align="center">
  <img width="1434" height="1017" alt="image" src="https://github.com/user-attachments/assets/63830f79-ba19-407c-8d9b-538fc896e6b1" />
</p>

```bash
# clone the Workshop collateral directory
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git

# cd into the directory
cd riscv_workshop_collaterals

# Check if the files are there using ls -ltr
ls -ltr

# cd into labs
cd labs

# change the permissions & run the script to simulate the c code
chmod 777 rv32im.sh
./rv32im.sh
```
<p align="center">
  <img width="1287" height="804" alt="image" src="https://github.com/user-attachments/assets/4bd2231b-4522-4bf1-9839-0630646beb96" />
  <img width="1290" height="344" alt="image" src="https://github.com/user-attachments/assets/c2d4e8b1-7962-49bd-950d-dda05743524a" />
</p>

</details>


## Day 3 - Digital Logic with TL-Verilog and Makerchip

<details>
<summary><b>RV Day 3</b></summary>

### D3SK1 Combinational logic in TL-Verilog using Makerchip
### D3SK2 Sequential logic
### D3SK3 Pipelined logic
### D3SK4 Validity
### up

</details>


## Day 4 - Basic RISC-V CPU Micro-architecture

<details>
<summary><b>RV Day 4</b></summary>

### D4SK1 Introduction to Simple RISC-V Micro-architecture
### D4SK2 Fetch and decode
### D4SK3 RISC-V control logic

</details>


## Day 5 - Complete Pipelined RISC-V CPU Micro-architecture

<details>
<summary><b>RV Day 5</b></summary>

### D5SK1 Pipelining the CPU
### D5SK2 Solutions to Pipeline Hazards
### D5SK3 Load/Store Instructions and Completing RISC-V CPU

</details>


