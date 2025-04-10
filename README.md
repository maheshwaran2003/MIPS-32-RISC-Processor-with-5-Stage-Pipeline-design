# Design of 5 Stage Pipelined MIPS32 RISC Processor  
This repository contains the details and the code for the MIPS32 ISA based RISC Processor, which is implemented in 5 stage pipelined configuration.  
## Table of contents  
▫️ [MIPS32](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-mips32)  
▫️ [Addressing Modes](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-addressing-modes)  
▫️ [Instructions considered](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-instructions-considered)  
▫️ [Instruction Encoding](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-instruction-encoding)  
▫️ [Stages of Execution](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-stages-of-execution)  
▫️ [Pipelined DataPath](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-pipelined-datapath)  
▫️ [Known issues and problems](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-known-problems-and-issues)  
▫️ [References](https://github.com/arpit306/5-Stage-Pipelined-MIPS32-RISC-Processor-Design-on-Verilog#%EF%B8%8F-references)  
## ▫️ MIPS32  
- 32 x 32 bit GPRs [R0 to R31]  
- R0 hardwired to logic0  
- 32 bit Program Counter (PC)  
- No flag registers (carry, zero, sign..etc)  
- Few Addresing Modes  
- Only Load and Store instructions can access memory  
- We assume memory word size is 32 bits (word addressable)  
## ▫️ Addressing Modes  
| Addressing Mode | Example Instruction |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Register addressing | ADD R1,R2,R3      |
| Immediate addressing | ADDI R1,R2, 200       |
| Base addressing      | LW R5, 150(R7)    |
| PC relative addressing  | BEQZ R3, Label   |
| Pseudo-direct addressing | J Label      |
## ▫️ Instructions Considered  
Not all instructions of MIPS32 are considered in this design, for implementation sake only a few instructions are considered, mentioned below:  
- Load and Store Instructions  
```
LW R2,124(R8) // R2 = Mem[R8+124]  
SW R5,-10(R25) // Mem[R25-10] = R5  
```
- Arithmetic and Logic Instructions (only register operands)  
```
ADD R1,R2,R3 // R1 = R2 + R3  
ADD R1,R2,R0 // R1 = R2 + 0  
SUB R12,R10,R8 // R12 = R10 – R8  
AND R20,R1,R5 // R20 = R1 & R5  
OR R11,R5,R6 // R11 = R5 | R6  
MUL R5,R6,R7 // R5 = R6 * R7  
SLT R5,R11,R12 // If R11 < R12, R5=1; else R5=0 
```
- Arithmetic and Logic Instructions (immediate operand)  
```
ADDI R1,R2,25 // R1 = R2 + 25  
SUBI R5,R1,150 // R5 = R1 – 150  
SLTI R2,R10,10 // If R10<10, R2=1; else R2=0 
```
- Branch Instructions  
```
BEQZ R1,Loop // Branch to Loop if R1=0  
BNEQZ R5,Label // Branch to Label if R5!=0  
```
- Jump Instruction  
```
J Loop // Branch to Loop unconditionally  
```
- Miscellaneous Instructioon  
```
HLT // Halt execution 
```
## ▫️ Instruction Encoding  
![ISR](https://user-images.githubusercontent.com/68592620/231771092-0c93aeb3-6b01-478f-a363-ecadb1ec578a.png)  
- shamt : shift amount, funct : opcode extension for additional functions.
- Some instructions require two register operands rs & rt as input, while some require only rs. 
- This requirement is only identified only after the instruction is decoded. 
- While decoding is going on, we can prefetch the registers in parallel, which may or may not be used later. 
- Similarly, the 16-bit and 26-bit immediate data are retrieved and signextended to 32-bits in case they are required later.  
## ▫️ Stages of Execution  
The instruction execution cycle contains the following 5 stages in order:  
1. IF : Instruction Fetch  
2. ID : Instruction Decode / Register Fetch  
3. EX : Execution / Effective Address Calculation  
4. MEM : Memory Access / Branch Completion  
5. WB : Register Write-back  
- micro operations not shown here.
## ▫️ Pipelined DataPath  
![pipelined](https://user-images.githubusercontent.com/68592620/231771102-12c05fa9-6e74-4835-abc6-1bd9b20e8453.png)  
## ▫️ Known problems and issues  
Following pipelining hazards are present in the given design :  
- Structural Hazards due to shared hardware.  
- Data Hazards due to instruction data dependency.  
- Control hazards due to branch instructions.  
## ▫️ References  
[NPTEL \& IIT KGP 'Hardware Modeling using Verilog'- Prof. Indranil Sengupta](https://nptel.ac.in/courses/106105165)
