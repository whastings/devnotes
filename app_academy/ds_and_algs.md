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

## Week 11 - Day 1

**Static Array**
- Have to choose a fixed max size at the beginning and it can't resize
- Usually have to declare type of data to store and only store that type
- Uses fixed, consecutive blocks of memory
- Needs to keep track of total & filled length and where in memory items start
- Lookup:
    + When you give computer an index to retrieve, it computes the memory location that holds it
        * e.g. `array[3]` is at `starting_pos` + (8 * 3)
        * This is *pointer arithmetic* and is very fast
- Assignment:
    + Finds memory location just like lookup, so also very fast
- Pushing & Popping:
    + Popping is fast
    + Pushing is fast
        * Always adds new element at `starting_pos` + (8 * `filled_length`)
            - Computer can calculate this super quickly, regardless of array size (constant time)

## Week 11 - Day 3

**Dynamic Array** 
- Like static array, but can resize when it gets full
- Resizing is slow
    + Have to copy everything to new, bigger memory location
    + Have to free old memory location to avoid memory leak
    + Usually doubles space on each resize
        * Keeps number of resizes to a minimum w/o wasting too much memory
        * Keeps ratio of resize cost to time to next resize constant
            - Cost of resize is proportional to length of array
            - The benefit is number of new spaces created
- Slow to insert between elements, as all elements after have to be moved over
- Push is usually fast, unless resize needed
    + Resize is a linear operation
    + But total resize time increases as array length grows

**Ammortization**: An amount spread out over a period of time

## Week 11 - Day 5

**Big O**
- a.k.a **Asymptotic Time Complexity**
- A way to classify algorithms based on how quickly they handle problems
- Represent size of problem by variable `n`
    + e.g. Number of items to process
- Compares things that grow at *different rates*, not those that grow at the same rate
- Represents a set of functions that *are within* the function described in the notation
    + By *within*, we mean that the Big O function (`f`) *dominates* the represented functions (`g`)
        * **Dominates** = One function's time grows faster than another's as problem size increases
        * So the function represented by the Big O notation *is the worst case scenario*
        * In Big O, `f` (e.g. `n^2`), *dominates/grows faster* than any of the functions that fall within its set/class

**Complexity Classes**:
- **Linear**: Grows at same rate as size of problem
    + a.k.a. **O(n)**
- **Exponential** 
- **Logarithmic**
    + Grows more slowly than problem size
- **Quadratic**
    + a.k.a. **O(n^2)**

Different types of problems (classes) grow in difficulty at different rates as number of items increases
- Some operations will grow in difficulty at faster rates
    + So may start out faster than another operation, but will overtake and take longer than other as problem size grows
- Can compare two operations with: `(time_op_1 * n) / (time_op_2 * n)`
    + Gives you a ratio
    + Approaches 0 as n approaches infinity

