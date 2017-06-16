# Book Notes: Growing Object-Oriented Software Guided by Tests

## Ch. 3: An Introduction to the Tools

* They'll be using JUnit and jMock2 to demo TDD in the book

## Ch. 4: Kick-Starting the Test-Driven Cycle

* Start a project by creating and testing a **Walking Skeleton**
  * Is the smallest possible bit of functionality that you can build, deploy, and test end-to-end
  * Helps you set up the infrastructure you'll need for your project early so you don't wait till
    the end to do it.
* End-to-end tests should also exercise the deployment steps
* "nothing forces us to understand a process better than trying to automate it"
* Take functional and non-functional requirements into account when designing initial structure
* Seek feedback as early and often as possible, as there's no guarantee your decisions are correct
  * Especially from real users
* Iterative development tackles chaos and uncertainty at the beginning of a project instead of
  leaving it until the end

## Ch. 5: Maintaining the Test-Driven Cycle

* Start each feature by writing failing acceptance tests
  * Then you'll know the feature is done when the tests pass
  * And you'll clarify what needs doing
* New acceptance tests shouldn't fail the build until their feature is done
  * But when feature is done, switch them on to catch regressions
* Start by testing the "simplest possible success case" to get an idea of how implementation will
  work
* Before writing implementation, watch test fail and make sure error message is correct
* "Unit-test behavior, not methods"
  * Tests are better at documenting object when they describe the features it provides rather than
    its methods
  * A feature might map to one or multiple method calls
  * Makes it easier to remember how to use the object when you come back later
* If code is hard to test, its design likely needs improvement
* You should write some integration tests to make sure there are no problems when your objects
  actually interact

## Ch. 6: Object-Oriented Style

* Group together code that will change for the same reason
  * e.g. Code to manage data, code to use data for business logic
* "The only way for humans to deal with complexity is to avoid it, by working at higher levels of
  abstraction."
* Object may have more than one responsibility if you can't describe it w/o using "and" or "or"
* An object should "exhibit simpler behavior than all of its component parts considered together"
  * Object should hide existence of objects it uses and expose an API that defines a simpler
    abstraction
* Context-Independence: An object should have no built-in knowledge of its environment
  * Makes it easier to reuse object in new situations
  * Anything an object needs from environment should be passed in (to constructor or methods)
  * Object is simpler because it doesn't have to define relationships
  * Relationships are made explicit
