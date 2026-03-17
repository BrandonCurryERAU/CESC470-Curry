The XRISC-32 ISA is designed with simplicity as a primary objective, targeting small to medium-sized embedded systems. It is a 32-bit architecture that uses fixed-length instructions, enabling efficient single-cycle execution similar to modern RISC architectures. This fixed instruction format reduces decoding complexity, which in turn improves pipeline efficiency compared to more complex CISC designs that often require multiple cycles per instruction.

The ISA supports standard instruction formats, including R-type and I-type instructions. Examples of R-type instructions include ADDR (register addition) and SUBR (register subtraction), while an example of an I-type instruction is LW (load word). Because of the streamlined design and reduced hardware complexity, XRISC-32 is well-suited for cost-sensitive systems, as it requires less complex and less power-hungry CPU implementations.

R-type instructions are used for register-to-register operations executed by the ALU. For example, the ADDR instruction adds the values stored in registers RT and RD, and writes the result to register RS.

**R-type ADDR / SUBR instructions:**\
opcode = 6 bits\
rs = 5 bits\
rt = 5 bits\
rd = 5 bits\
shamt(shift instruction) = 5 bits\
funct = 6 bits

I-type instructions are meant for immedicate operations or load/store operations. The LW instruction either adds an immediate value (imm) into register RS, or loads the value of register RT into register RS.

**I-Type LW instruction:**\
opcode = 6 bits\
rs = 5 bits\
rt = 5 bits\
imm = 16 bits (for immediate arithmetic)

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
