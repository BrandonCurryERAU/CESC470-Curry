# My processor: The XRISC-32 ISA

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

**Example Binary Encodings:**

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

# Diagrams
**Single Cycle Diagram:**

<img width="1195" height="520" alt="544081320-554ed445-7806-4fb3-9fac-93966a64ef37" src="https://github.com/user-attachments/assets/a4d64b39-44ef-4e53-9788-8bc278532b72" />

**Pipelined Datapath Diagram:**

<img width="1198" height="474" alt="553862193-a15e9334-5cb4-4e21-b073-0616a43e2556" src="https://github.com/user-attachments/assets/fab20fa2-2b51-461a-ae24-4269a01d80e5" />


**Pipeline Modifications** 

For this modification, two multiplexors were added with a forwarding / bypassing pathway after ALU execution to prevent from data stalls incase of a data hazard. This can be caused if an instruction is performed which requires the value of a register from an arithmetic instruction immediately before it. Adding this forwarding method helps to provide the value directly to the next instruction so there wouldn't be a delay waiting for the new value to be loaded into the register. The diagram is shown below for the change:

<img width="1000" height="439" alt="imageedit_1_2373879458" src="https://github.com/user-attachments/assets/b1524be0-cbe6-4791-a5a2-13f151dcf72c" />

**Processor Memory Hierarchy and Cahce Behavior**

For my memory hierarchy, my configuration is set to these specifications:

**L1 Cache:**
Size: 64 KB
Associativity: 4-way set associative
Block size: 16 bytes
Latency: about 1 cycle

**L2 Cache:**
Size: 256 KB
Associativity: 4-way set associative
Block size: 16 bytes
Latency: about 1–2 cycles

**Main Memory:**
Size: 16 GB RAM
Latency: about 4 cycles

For this example, the cache access pattern will be a sequential array tranversal. 

```
for (int i = 0; i < 8; i++) {
    total += A[i];
}
```

Each 16-byte block holds 4 elements.

As each integer is implemented into each block, a cache behavior table is shown which shows the hit/miss patterns:

| Access # | Address | Block # | L1 Result | L2 Result | Reason                   |
| -------- | ------- | ------- | --------- | --------- | ------------------------ |
| 1        | 0x00    | 0       | Miss      | Miss      | Cold miss (first access) |
| 2        | 0x04    | 0       | Hit       | —         | Same block               |
| 3        | 0x08    | 0       | Hit       | —         | Same block               |
| 4        | 0x0C    | 0       | Hit       | —         | Same block               |
| 5        | 0x10    | 1       | Miss      | Miss      | New block                |
| 6        | 0x14    | 1       | Hit       | —         | Same block               |
| 7        | 0x18    | 1       | Hit       | —         | Same block               |
| 8        | 0x1C    | 1       | Hit       | —         | Same block               |
| 9        | 0x20    | 2       | Miss      | Miss      | New block                |
| 10       | 0x24    | 2       | Hit       | —         | Same block               |
| 11       | 0x28    | 2       | Hit       | —         | Same block               |
| 12       | 0x2C    | 2       | Hit       | —         | Same block               |

Grabbing from these results, there are 3 hits in every 4 accesses which means a hit rate of 75% and a miss rate of 25%. This also produces an AMAT of 1.5 cycles since L1 has a miss rate of 25% and the worst case hit rate of L2 is 2 cycles.

