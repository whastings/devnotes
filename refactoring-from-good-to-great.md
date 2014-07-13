# Refactoring from Good to Great by Ben Orenstein
https://www.youtube.com/watch?v=DC-pQPq0acs

* Breaking code out into a private method makes the method it came from easier
  to read
  * Lets the reader know that the private method contains implementation details
    they don't need to know about
* **Tell, Don't Ask**: Generally better to send object a method call when you
  want something than to inspect its internal state and decide what to do with it
  * Bad: `order.placed_at >= start_date && order.placed_at <= end_date`
  * Good: `order.placed_between?(start_date, end_date)`
  * Keeps internal details from leaking out
* **Data Clump**: Code smell characterized by two or more arguments that are
  repeatedly passed around together
  * Would be better to create an object to hold those values
* Low coupling makes code easier to change
  * Can change one class without breaking another
* In Ruby, `Range#cover?` is faster than `Range#include?` because `include?`
  creates all objects in range and checks them all
* **Feature Envy**: Code smell in which one class is interacting a lot with the
  internals of another class
* **Null Object Pattern:** A special class to stand in for the absence of an
  object instead of `nil` or `null`
  * Implements same API as real class
  * Can return defaults for common queries
    * e.g. `NullPerson#name` returns 'no name'
  * Makes it so class using it can avoid conditionals testing if it has an
    instance or not
* Things to refactor:
  * God objects: Objects that many other parts of the program rely on
  * High-churn files: Ones that you're changing a lot
  * Where you find bugs ("bugs love company")
* Recommendations:
  * *Clean Code* by Robert Martin
  * *Refactoring* by Martin Fowler
  * *Growing Object-Oriented Software Guided by Tests*
