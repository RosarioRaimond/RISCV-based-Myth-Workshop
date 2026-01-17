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
<summary><b>Topics Covered</b></summary>

</details>


## Day 3 - Digital Logic with TL-Verilog and Makerchip

<details>
<summary><b>Topics Covered</b></summary>

</details>


## Day 4 - Basic RISC-V CPU Micro-architecture

<details>
<summary><b>Topics Covered</b></summary>

</details>


## Day 5 - Complete Pipelined RISC-V CPU Micro-architecture

<details>
<summary><b>Topics Covered</b></summary>

</details>


