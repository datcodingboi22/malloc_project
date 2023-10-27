# malloc_project
Please upload only the new mm.c file once you are done
here is some extra information:

Implementation

The mm.c file contains declarations which define the following structures, which you do not need to modify:

typedef struct _BlockInfo {
  // Size of the block and whether or not the block is in use or free.
  // When the size is negative, the block is currently free.
  long int size;
  // Pointer to the previous block in the list.
  struct _Block* prev;
} BlockInfo;

typedef struct _FreeBlockInfo {
  // Pointer to the next free block in the list.
  struct _Block* nextFree;
  // Pointer to the previous free block in the list.
  struct _Block* prevFree;
} FreeBlockInfo;

typedef struct _Block {
  BlockInfo info;
  FreeBlockInfo freeNode;
} Block;

/* Pointer to the first FreeBlockInfo in the free list, the list's head. */
static Block* free_list_head = NULL;
static Block* malloc_list_tail = NULL;
The important structure that you will use is the Block structure. This contains the metadata for your block.

The info field of the structure is the metadata that is a part of every block on the heap, free and allocated alike. The freeNode field is metadata that only a free block has.

As a space optimization, you will allow the user program to write over the freeNode information. In memory, the info struct comes before the freeNode section in memory. If you placed the Block structure in memory, then if you add sizeof(BlockInfo) to a pointer to a Block structure, it will be the address of the FreeBlockInfo structure. It is this address that should be returned by mm_malloc to make the best use of space.

You can ignore pointer arithmetic and add directly to a pointer using the UNSCALED_POINTER_ADD macro we provided.

In this case, if you have a pointer to a Block called ptrFreeBlock and you want mm_malloc to return the first address that a program can safely write to, you can write:

return UNSCALED_POINTER_ADD(ptrFreeBlock, sizeof(BlockInfo))
Free Blocks

In this assignment, we are going to treat free blocks as being any Block that has a negative size. An allocated block, then, is a Block that has a positive size.

Here is some code that will determine if a Block is free:

Block* ptrBlock = first_block();

if (ptrBlock->info.size < 0) {
    // Free Block
}
Notice the ptrBlock->info arrow syntax dereferencing the pointer to a Block structure. Yet, since the info field is not a pointer, we directly refer to the fields within. Therefore, info.size uses a dot syntax.

Makefile:
make
If there are no errors, the compiler will generate several programs. One program is mdriver which will test your implementation against some exhaustive program traces. Documentation on commands can be seen by running the mdriver with the -h flag:

./mdriver -h
The following file (traces/short1-bal.rep) illustrates an example trace:

20000
6
12
1
a 0 2040
a 1 2040
f 1
a 2 48
a 3 4072
f 3
a 4 4072
f 0
f 2
a 5 4072
f 4
f 5
You can mostly ignore the first few lines. Lines that start with a will call mm_malloc with the size passed being the last number of that line. The lines starting with f will call mm_free with the address returned by the corresponding mm_malloc call. That is, the prior a line of the trace with the same identifying number (the second token on every a or f line).

You can write your own trace by copying an existing one. To use any of the provided traces, just use mdriver like this:

./mdriver -f traces/short1-bal.rep
If there are no errors, it will print only one line which shows the relative performance of your implementation.

You can run all traces using:

./mdriver
Or verbosely using:

./mdriver -v
Generally, with the initial phase, you will be in the 40s running all traces. Your final implementation in the final phase should be in the high 60s when running all traces. It may deviate due to the server load.

Please make sure it works fully before submitting final product. 

You may use any of the other provided and fully implemented functions to help you implement the functions you need to fix. 

Code can be tested using trace files which are files ending in .rep

