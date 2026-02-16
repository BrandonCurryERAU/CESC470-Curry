My ISA is named XRISC-32 and is designed for simplicity and meant for smaller embedded-systems for small-medium sized devices. The XRISC-32 ISA is 32-bit width and has fixed-size intructions to be operating in single cycles, much like the commercially used RISC out today. An example of some R-Type instruction types are ADDR (Add registers), SUBR (subtract registers), and an example of a I-type instruction LW (Load Word). With a fixed-instruction set, pipelining will be efficient since there is small decoding complexity unlike CISC, which also requires multiple cycles. Since the ISA is also built for smaller systems, hardware cost will have an advantage since the ISA will be for less complex /powerful CPU chips that perform simple tasks.

R-Type instructions are meant for register operations performed by the ALU.

R-type ADDR / SUBR instructions:\
opcode = 6 bits\
rs = 5 bits\
rt = 5 bits\
rd = 5 bits\
shamt(shift instruction) = 5 bits\
funct = 6 bits\

I-type instructions are meant for immedicate operations or load/store operations. 

I-Type LW instruction:\
opcode = 6 bits\
rs = 5 bits\
rt = 5 bits\
imm = 16 bits (for immediate arithmetic)\

ADDR R1, R2, R3 

| opcode (6) | rs (5) | rt (5) | rd (5) | shamt (5) | funct (6) |
|----------|----------|----------|----------|----------|----------|
| 000000 | 00010 | 00011 | 00001 | 00000 | 100000 |

SUBR R1, R2, R3

| opcode (6) | rs (5) | rt (5) | rd (5) | shamt (5) | funct (6) |
|----------|----------|----------|----------|----------|----------|
| 000000 | 00101 | 00110 | 00100 | 00000 | 100010 |

LW R3, 4(R2)

| opcode (6) | rs (5) | rt (5) | immediate (16) |
|----------|----------|----------|----------|
| 001000   |  00010 |  00001 | 0000000000001010 |
