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

## Ch. 7: Achieving Object-Oriented Design

* We write tests first so we can:
  * Know what we want to achieve
  * Know what dependencies of object will be
    * A unit test is just another context for an object
* Object communication patterns are more important than class structure
* Create **Value Types** that are immutable objects that model value concepts in your domain
  * Are closer to primitive types than regular OO objects
  * Values don't have changing state and so don't have any meaningful identity
  * Don't have to have much behavior
  * e.g. Create an `ItemType` value type instead of just using a string
* When a group of values are always used together, consider creating a new type to bundle them up
* Methods for discovering new objects in your design:
  * Breaking Out: Pull cohesive bits of functionality out of a complex class into smaller
    collaborating objects
    * Then can test smaller objects independently
  * Budding Off: Create a new object to act as a service for an existing object when you find the
    existing object needs some functionality that doesn't belong inside of it
    * Use interface to abstract the role the new object plays for the old object
    * Discover new types by "pulling them into existence"
    * Process can repeat in finding services the new object needs
    * "We think of this as “on-demand” design: we “pull” interfaces and their implementations into
      existence from the needs of the client, rather than “pushing” out the features that we think a
      class should provide."
  * Bundling Up
    * When a cluster of objects work together, you can bundle them all up into a containing object
    * Containing object can hide and abstract the contained objects
* Use interfaces to describe the roles objects can play
  * Prefer having many narrow interfaces over few large interfaces
    * Keeps each role well-defined
* "We aspire to raise ourselves from programming in terms of control flow and data manipulation, to
  composing programs from smaller programs—where objects form the smallest unit of behavior."

## Ch. 8: Building on Third-Party Code

* Prefer to avoid mocking third-party code
  * If you mock, you might not get the behavior of the third-party code exactly right
  * Sometimes you have to mock hard-to-trigger behavior
* Write adapter objects to translate between your application domain and third-party code
  * Adapter objects can present exactly the services that application objects need
  * Helps keep low-level technical details out of the application code

## Chapter 9: Commissioning an Auction Sniper

* Learn to slice up functionality into pieces that can be done a little at a time
  * Should focus on one thing you can achieve quickly
* Will build a program to bid on auction over the Internet
  * Will receive latest bid plus required increment from service and make another bid if it's losing
  * Will build as a Java Swing app communicating over XMPP

### Chapter 10: The Walking Skeleton

* Will start by getting the app to connect to an auction and display text in a label in the UI
* Build your infrastructure to support the way you want to test
  * Don't let infrastructure dictate how you test
  * Takes a while, but is worth it
* Write e2e tests in terms of the business domain, not in terms of the UI or other tech details
  * Hide UI details in test helpers
  * Then tests can stay the same when implementations change
* Write your first e2e test before anything else to help you make structural decisions about your system and deploy process
* e2e tests are tricky because they run separately from system and can't peek inside it
  * Must wait for some detectable effect
  * Have to deal with asynchrony

## Ch. 11: Passing the First Test

* Create `ApplicationRunner` to handle running the app in test
  * Will tell the app what to do plus make assertions about state of UI
* Also build a fake auction that emulates the real one plus makes assertions about messages received
* You can write "ugly" code to test out your ideas, but don't leave it that way
* Don't sweat design decisions too hard at this early stage

## Ch. 12: Getting Ready to Bid

* Get app to send a big to the auction
  * Auction sends price then app responds with next bid
  * App shows it's bidding after receiving price, then shows it has lost when auction says it closed
* When testing and developing, start with triggering and outside event and looking for a visible
  effect
  * "Our approach to test-driven development is to start with the outside event that triggers the
    behavior we want to implement and work our way into the code an object at a time, until we reach
    a visible effect (such as a sent message or log entry) indicating that we’ve achieved our goal.
    The end-to-end test shows us the end points of that process, so we can explore our way through
    the space in the middle."
* Create an `AuctionMessageTranslator` class to listen for events from XMPP chat then fire new
  events in the app's domain
  * Implements `processMessage`, calls `auctionClosed` and `currentPrice`
  * Will send events to object implementing `AuctionEventListener` interface
  * Then have `Main` implement `AuctionEventListener` so it can update the UI

## Ch. 13: The Sniper Makes a Bid

* Create `AuctionSniper` class to respond to price events
  * Replaces `Main` as the implementer of `AuctionEventListener`
    * Moves complexity out of `Main`
  * But don't want it concerned with the UI
  * So create a new `SniperListener` interface
    * `AuctionSniper` will call a listener when UI needs updating
      * Will call `sniperBidding`
    * `Main` will implement `SniperListener` and continue handling UI updates
  * Keep striving to follow Single Responsibility
* Create an `Auction` to make chat requests on behalf of `AuctionSniper`
  * `Auction` is a dependency whereas `SniperListener` is a notification
  * Will implement `bid` and `join` for sniper to call
  * `Auction` is interface, and `XMPPAuction` is created to implement it
* When writing expected value in test, balance readability and maintainability
  * "We've specified the expected bid value by adding the price and increment . There are different
    opinions about whether test values should just be literals with “obvious” values, or expressed
    in terms of the calculation they represent. Writing out the calculation may make the test more
    readable but risks reimplementing the target code in the test, and in some cases the calculation
    will be too complicated to reproduce."
* Move UI handling out of `Main` by creating `SniperStateDisplayer` class that implements
  `SniperListener`
* Create `AuctionEvent` class inside `AuctionMessageTranslator` to encapsulate parsing auction
  messages from chat
  * Is a "value type"
  * Is an example of "breaking out"
* Sometimes create a "null implementation" of a method or class so you can continue to make progress
  and keep focusing on the immediate task
  * e.g. Creating a fake implementer of an interface in another object's unit test
* Be prepared to defer refactoring code when it's not yet clear what to do

## Ch. 14: The Sniper Wins an Auction

* Make sniper bid and then win
  * Will show winning when latest bid is its own
  * Will show it has won when auction then closes
* Create `PriceSource` enum with `FromSniper` and `FromOtherBidder` values so
  `AuctionMessageTranslator` can tell sniper if it is the current winning bidder or not
* Then update sniper to show it is winning or to make another bid based on `priceSource`
* Add state to sniper of whether it is currently winning or losing so it can react appropriately
  when auction closes
  * Will call either `sniperLost` or `sniperWon` on the `SniperListener`
  * Create `isWinning` boolean instance variable
