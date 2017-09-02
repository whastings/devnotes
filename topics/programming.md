# Programming Notes

## OO Design

* Methods should be commands or queries, not both
  * Query returns something and has no side effects
  * Command triggers side effects and returns nothing
  * Test return values of query; side effects of command 

## Best Practices

* KISS: Keep it simple, stupid
  * Do the simplest thing that could possibly work
  * "Do only what is necessary to solve the problem at hand, and no more. Over-design is a house of cards, and if you can barely understand it when you're building it, it will all come crashing down when you're trying to maintain it six months from now. Do yourself, or the next programmer that has to look at your code, a favor and write it as cleanly and concisely as possible, and keep it simple" -Sam Koblenski
* YAGNI: You ain't gonna need it
* Occam's Razor
  * Pursue the simplest explanation first
  * Don't assume a bug is in library or platform code instead of your own
* Amdahl's Law: Your program can only be as fast as it's slowest component
  * So focus on optimizing the slowest parts
  * Don't spend time making optimizations that aren't significant
* Don't Let Standards Make You Stupid
  * "while standards are generally a good thing, they should not preclude you from thinking about the problem at hand and solving it in the best way possible. Standards can add some uniformity and consistency to programming and offer known solutions to common problems, but standards can also go to far. They can become rigid structures that mask over issues that are unique to particular problems that require their own creative thinking" -Sam Koblenski

## Testing

* Keep tests readable by keeping code to a minimum
  * Write helpers outside the tests to abstract setup and other details that are generic
  * Put repeated expectations in functions to keep things DRY
* Keep tests isolated from other tests
  * Don't let them share the same state

## Pragmatism

* Software exists to solve business problems in order to decrease costs and increase revenue
  * Not to please programmers
  * Trouble happens when programmers use tools they don't need for problems they don't have
  * Always look for business benefit before changing or adding code
* Don't get too attached to any particular technology
* Let your code grow organically, only solving problems that come up
* The more you understand the business and problem domain you work in, the more effective you will be
* Pay close attention to design and requirements, as they define the problem you're solving
* Focus on solving problems and only optimize when you need to
* Don't try to learn everything; learn as the need arises
* See:
  * http://lucasfcosta.com/2017/07/17/The-Ultimate-Guide-to-JavaScript-Fatigue.html

## Abstractions

* [Cost of Abstractions][cost-abstractions]
  * Cost more to implement than direct approach
  * Are flexible, while direct approach is rigid
  * Have overhead of additional complexity
    * Not worth it if only used once
  * Are better for parts of system that change less often
  * When to abstract
    * "Abstractions only work well in the right context, and the right context develops as the system develops."
    * "The right time to add an abstraction to a design is at the point when you start feeling the pain of not having it. Don't do it sooner because it's quite possible the extra work will be wasted and the extra complexity will be a burden. Don't wait too long because the whole time you're feeling the pain of not having the abstraction, more and more work is piling up that will have to be done to switch over to the abstraction."
      * Otherwise you end up with an overly complex system that is full of unneeded abstractions that do more harm than good
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
[cost-abstractions]: http://sam-koblenski.blogspot.com/2014/07/the-cost-of-abstraction.html?m=1
[min-api-surface]: https://2014.jsconf.eu/speakers/sebastian-markbage-minimal-api-surface-area-learning-patterns-instead-of-frameworks.html
