# Algorithms - Notes

## Techniques

### Divide and Conquer

* Involves recursively breaking a problem into sub-problems until the sub-problems become simple
  enough to solve directly, then combining their solutions to get the solution to the original
  problem

### Dynamic Programming

* An extension to Divide and Conquer that uses memoization or tabulation
* Requires a problem to have overlapping sub-problems when broken down
  * One sub-problem needs to be solved multiple times
* Involves storing the solutions to sub-problems so they can be reused without solving them again
  * In order to boost performance
* **Memoization:** Caching and reusing previously computed results
* **Tabulation:** Filling up a cache from the bottom and reusing previous results
  * e.g. Computing Fibonacci numbers by filling up an array, looking at previous values stored in
    the array to compute next values
* Example applications:
  * Fibonacci calculations
  * Levenshtein distance
