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

## Week 11 - Day 5 through Week 12 - Day 1

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
- **Constant Time**: Takes same amount of time regardless of problem time (fixed cost)
    + a.k.a. **O(1)**
    + e.g. Array index lookup, lookup in a Hash-based Set
    + *Has the best performance*
- **Logarithmic Time**
    + a.k.a. **O(log(n))**
    + Grows more slowly than problem size
    + e.g. Binary search, binary trees
    + *Has great performance*
        * If you double problem size, you only do one more unit of work
- **Linear Time**: Grows at same rate as size of problem
    + a.k.a. **O(n)**
    + e.g. a loop
    + *Has fair performance*
        * If you double problem size, it's twice as hard
- **Quadratic Time**
    + a.k.a. **O(n^2)**
    + e.g. Pairs, loops inside loops
    + May be able to improve by sorting
        * e.g. Using two loops to solve the Two Sum problem is quadratic
            - Could sort the array of numbers
            - Then search with binary search for a number that produces 0 when added to current number
            - Which gives you O(n log(n)) performance
            - Can get even better (O(n)) by using a Hash for number lookup
    + *Has poor performance*
        * If you double problem size, it's four times as hard
- **Exponential Time**
    + a.k.a. **O(2^n)**
    + e.g. Subsets
    + *Has terrible performance*
        * If you increase problem size by one, it's twice as hard 
- **Factorial Time**
    + a.k.a. **O(n!)**
    + e.g. Permutations
    + *Has the worse performance*

Different types of problems (classes) grow in difficulty at different rates as number of items increases
- Some operations will grow in difficulty at faster rates
    + So may start out faster than another operation, but will overtake and take longer than other as problem size grows
- Can compare two operations with: `(time_op_1 * n) / (time_op_2 * n)`
    + Gives you a ratio
    + Approaches 0 as n approaches infinity

**Multi-Class Problems**: The solutions for some problems are composed of smaller operations that have different complexities
- **O(n(log(n)))**: Part of solution is linear, part is logarithmic
    + Can come up in a sorting algorithm like *merge sort*
    + *Has okay performance*
    + Can improve to linear time by avoiding sorting

## Week 12 - Day 3

The best performance you can get with sorting is O(nlog(n))
- Bubble Sort is O(n^2)
    + Can optimize, for example, by checking one less element from the end on each round
        * But can never get better than O(n^2)
- Merge Sort is O(nlog(n))
    + The splitting part is O(log(n))
    + The merging part is O(n)
        * Uses a *two-finger merge*
- Quick Sort is O(n^2) at its worst case
    + BUT, it tends to average O(nlog(n)) performance

## Week 12 - Day 5

**Hash Sets**: Collection of unique items, implemented as a hash
- Stores data in buckets in an array, with mechanism for determining which bucket an item belongs in
    + e.g. If you're storing ints, you could do `num % buckets.length` to determine the bucket the number goes in
        * Is constant time to find the right bucket
        * For objects, could do `object.hash % buckets.length`
    + Ideally, you have one item per bucket
        * Finding the bucket is fast, but then you have to linearly go through each item in the bucket to find one you want
- When buckets start getting too full, the hash sets need to resize itself by doubling the number of buckets
    + Have to move all items to new correct buckets
    + Good time to resize is when number of items equals bucket array's length
    + Resize is slow (O(n)), which is why you double # buckets each time
        - Tends to break up large buckets
        - Since you double, don't have to resize often, which *ammortizes* cost of resizing over life of the set
        - Resize takes longer each time, but you get a longer time till the next resize each time
- Worst Case: Everything is in the same bucket
    + But on average, each bucket will have `num_items / num_buckets` items
- Doing `include?` is, on average, O(1)
    + Is constant time to find bucket, then linear to search bucket's array
    + But on average, buckets will be small because you'll have `num_buckets - 1` or less items
- In production, hash set will use a *hash algorithm* to further scramble the item and make the bucket it ends up in *more random*

## Week 14 - Day 1 - Day 3

**Hash Function**: Algorithm to produce a *hash* of an input with the goal of all elements being evenly distributed
- Reduces the likeliness of **hash collisions** (two items in the same bucket)
- For even distribution, needs to be *suitably random*, while still producing the same output consistently for a given input
- Can combine hash's by `XOR`ing them
    + e.g. `@name.hash ^ @age.hash`
    + Can be useful when creating a custom hash method that combines all hashes of your object's instance variables
    + Also good for producing hash of an array by combining hashes of all of its elements
- Properties of good hash functions:
    + Probability of any bit being 1 or 0 is 50%
        * Means results will be evenly distributed
- Can hash a string because its basically an array of character codes (numbers)
- In Ruby:
    + For an object to be a valid hash key, its class must:
        * Override `==` in a way that defines *equality* for that class
        * Override `hash` in a way that produces the same value for two instances that `==` each other
            - Can return the hash of an array of all your relevant instance variables

**Queue**: Abstract data type that's *first-in, first-out*
- Linked Lists are good for these

**Priority Queue**: Queue in which elements are removed based on their *importance*, not the order in which they were added
- Keeps elements in order sorted by importance
- e.g. A CPU giving priority to certain processes when time slicing

**Heap**: A tree data structure
- A heap must be *full*: All nodes must be *left-aligned* (fill up nodes to the left before moving to the right)
    + Holes can only appear on the right of the final row
    + Because of this property, you can store elements of a heap in a dynamic array instead of in a tree node for better time and space efficiency
        * You can find element's left child with `2 * index + 1` and it's right with `2 * index + 2`
            - Providing each element only has two children max
- **Max Heap**: Heap in which a parent node's value is greater than or equal to those of its children
    + Biggest element is always at the root
    + **Peek**: Operation that looks at the largest element
        * Is O(1)
- **Min Heap**: Heap in which a parent node's value is less than those of its children
    + Smallest element is always at the root
- **Heapify Op**: Operation that resorts the heap on insert to preserve the constraints of the max heap
    + When child is greater than parent, switches child and parent, repeating until greatest element is at root
- *Insert* is proportional to depth of the tree
    + Is O(depth), which is O(log(n))
- **Heap Sort**:
    + Is an in place sort
    + Starts with an unordered array, then orders elements into a heap going from left to right
        * Does a heapify op each time it adds an element to the *heap end* of the array so elements will be in heap order at the end
    + Is O(n(log(n)))

**Sorting Algorithm Comparisons**:
- Bubble
    + Worst time: O(n^2)
    + Extra memory: O(1)
- Merge
    + Worst time: O(n(log(n)))
    + Extra memory: O(n)
    + Better than quick sort when consistent performance required
- Quick
    + Worst time: O(n^2)
    + Average time: O(n(log(n)))
    + Extra memory: O(log(n))
    + Better memory usage than merge sort
    + Faster than merge sort on average
- Heap
    + Worst time: O(n(log(n)))
    + Extra memory: O(1)
    + Tends to be slower than quick sort but faster than merge sort
        * Is more reliable than quick sort

**LRU Cache**
