# Data Structures & Algorithm Notes by Will Hastings

## Week 10 - Day 3

**Abstract Data Type**: A general notion of a process for storing and accessing data
- e.g. Sequence
    + Would have methods like indexOf, push, pop, shift, unshift, etc.
- Example implementations: Linked list, dynamic array
- Implementations have advantages and trade offs
    + Which is better depends on your situation
- e.g. Set
    + Would have methods like include?, insert, remove
    + Can be implemented like an array, as a tree, like a hash
- e.g. Map/Hash
    + Would have methods like keys, values, etc.

In binary arithmetic, you carry the 1 to the next digit if you get to 2
- You can convert 

**Computer Memory**:
- Can visualize as a list of cells
    + Each cell is usually 8 bytes (64 bits) wide
        * a.k.a. The *word size*
- Floating points are stored similarly to scientific notation
    + Decimal number in first 56 bits, exponent in remaining 8
- Storing signed numbers involves using first (most left) bit to indicate positive or negative, with rest of bits notating number
    + One byte can store from -128 to +127 of signed integers
    + One byte can store from 0 to 255 of unsigned integers
        * Is why parts of IP address can only go up to 255 (is part is one byte)
- Characters are all assigned a number (depending on encoding) to store in memory
    + ASCII goes from 0 to 255, needing one byte per character
- Memory location doesn't have an indication of what type of data it's storing
    + Instructions to computer tell it what type of data to interpret the memory's value as
- CPU handles
    + Arithmetic, exponentiations, logarithms
- Memory locations have **memory addresses**
    + Is how you tell the computer which pieces of data to use
    + A memory location can store the address of another memory location
        * These are **Pointers**
- Each operation on something in memory takes *1 unit of time*
    + e.g. Read one memory location, read a second, add them together, save result to third location

## Week 10 - Day 5

More memory:
- Allocating Memory:
    + In C, you use `malloc` to get memory, and it returns starting address
    + Fragmentation: When there aren't enough contiguous cells in memory to fullfil a request
        * Stack allocation: Computer avoids this by deallocating freed memory in order from one end to another
            - Has a stack pointer the points to the front of the stack
            - Objects must be deallocated in correct order
        * Heap allocation: Keeps notes of where free space ranges are
            - Objects can be deallocated in any order
            - But has more to keep track of
    + So Ruby uses heap allocation to keep track of memory references to actual data in stack-allocated memory

**Static Array**
- Have to choose a fixed max size at the beginning and it can't resize

**Dynamic Array** 
