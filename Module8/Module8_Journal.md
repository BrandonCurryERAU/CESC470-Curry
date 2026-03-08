Here is a code example in C++ of adding integers into an array sequentially with no N-amount of strides involved. 

'''for (int i = 0; i < 8; i++) {
    total += A[i];
}'''

Assume each integer size is 4 bytes, and each cach block size is 16 bytes. The cache is also originally empty upon clearing of cache memory. Since each block size is 16 bytes and each integer is 4-bytes, the total amount of integers each block can hold is 4 integers. Below is a breakdown of how much block will hold these integers:

Block 0 → A[0] A[1] A[2] A[3]
Block 1 → A[4] A[5] A[6] A[7]

Since each block is loaded into first-level L1, the first A[0] will be a miss since the block is originally loaded into L1 from main memory and will result in a compulsory miss as it goes through L1, L2, and DRAM. A[1] A[2] A[3] will be hits since each increment is within the same cache block and spatial locality allows for these to be done quickly since they are adjacent to eachother. A[4] will be a miss since it moves on to the next block (compulsary miss) and it must be added to L1 again from main memory which also goes through L2. A[5] A[6] A[7] will also be hits since the same rules apply from the first block and spatial locality will be utilized, using both L1 and L2 without needing to access main memory unless a new block is needed. 

In total, there are 8 access with 6 hits and 2 misses, which equates to a Hit Rate of 75% and a Miss Rate of 25%. 
