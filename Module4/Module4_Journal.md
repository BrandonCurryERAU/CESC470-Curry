**Datapath Diagram**

<img width="1195" height="520" alt="Single-Cycle Datapath ADD" src="https://github.com/user-attachments/assets/554ed445-7806-4fb3-9fac-93966a64ef37" />

**Datapath explanation**

This single-cycle datapath will be performing the operation ADD R1, R2, R3. The Program Counter is responsible for loading the next instruction to be executed, and the instruction address comes from there first. The Instruction Memory holds the current instruction being executed from the PC and has the instructions ADD, rd, rs, rt, and ALUop which will be used to execute the operation. The input for the ALU is grabbed from the Register File, where registers holding the data of R1, R2, and R3 are kept. The ALU will take the inputs of R2 and R3 and perform an addition operation with ALUSrc being "0" meaning the second operand comes directly from another register. Once the operation is complete, the output is stored in Data Memory and is written back to Register R1 through the WriteData command.
