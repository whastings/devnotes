# Programming Notes

## Beware Abstractions

* [Minimal API Surface Area - Learning Patterns Instead of Frameworks][min-api-surface]
  * Prefer code that's repetitive and even verbose but explicit, using few abstractions
    * This is more code but easier to understand and change than implicit
    * Avoid sugar
  * Only add new abstraction when it solves bugs
    * Make sure the abstraction is worth it's weight and doesn't cause more problems than it solves
    * Don't generalize repetition if it's not causing bugs
    * e.g. DOM update abstractions are good because manual update code is buggy
      * But things like Angular and Ember add too many abstractions
      * React tries to add as little as possible
    * Saving typing is not a good enough reason for a new abstraction
  * "It's easier to recover from no abstraction than the wrong abstraction"
  * Prefer language's standard API over competing, diverging libraries
    * Then there's no bikeshedding over libraries
  * Prefer patterns instead of frameworks or blackbox libraries
    * Your code will be easier to understand
  * The more abstractions you add, the more new folks have to learn to contribute
  * Rethink and purge unneeded things from code whenever necessary

## Beware Dogma

* "A dogma is born when the solutions are presented without enough original context, and newcomers feel pressured to delegate crucial decisions to an authority." -Dan Abramov, [The Case for Flux][case-for-flux]

[case-for-flux]: https://medium.com/swlh/the-case-for-flux-379b7d1982c6
[min-api-surface]: https://2014.jsconf.eu/speakers/sebastian-markbage-minimal-api-surface-area-learning-patterns-instead-of-frameworks.html
