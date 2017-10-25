# Book Notes: Growing Object-Oriented Software Guided by Tests

## Ch. 1: What Is the Point of Test-Driven Development?

* It's important to deploy completed work frequently so you can get feedback
  * Will help you check assumptions against reality, measure progress, find errors, and adapt your
    plan in response to what you learned
* Incremental/Iterative Development: Build each feature as a working, end-to-end slice
* Write code that's simple to understand and modify
  * Code is read more than written, so optimize for that
  * Will make handling unanticipated changes easier
* TDD gives feedback on code's design quality
* Writing testable code leads to more modular code and loosely coupled so you can test it in
  isolation
* TDD makes you figure out the definition of "done" for your next piece of work before you start it
* Start a feature by writing an acceptance test
  * Defines when the feature it done
  * Can't just have unit tests, as then you won't know if all the parts can work together
  * Can have acceptance tests not be able to fail a build until the feature they're testing is
    completed
  * Should only interact with the code from the outside, not call internal methods
  * Ideally, will also exercise the build and deploy process of the code
* Integration tests see if your code works with code you can't change
  * e.g. A third-party library
  * Good for testing abstractions you build around external code
* If it's hard to write a test for a class, there's probably an issue with its design
  * e.g. It has too many dependencies on distant things

## Ch. 2: Test-Driven Development with Objects

* OO design is more about communication between objects than the objects themselves
* "An object-oriented system is a web of collaborating objects. A system is built by creating
  objects and plugging them together so that they can send messages to one another. The behavior of
  the system is an emergent property of the composition of the objects—the choice of objects and how
  they are connected"
* Can change an OO system by changing the composition of its objects
* Be careful to distinguish value types from object types
  * Value: Models an unchanging quantity or measurement
    * Have no useful identity, so two instances with the same data are basically the same thing
  * Object: Container of mutable state that models behavior over time
    * Two instances have distinct identities even if at the moment they have the same state
  * It's confusing because in most languages both are implemented by classes
* Objects are interchangeable when they follow common communication patterns and have explicit
  dependencies
* Tests help you see the communication between your objects
* Roles, Responsibilities, & Collaborators
  * Responsibility: An obligation to perform a task or know some info
  * Role: A set of related responsibilities
    * An object can play one or more roles
    * In strongly typed languages, roles are often represented by interfaces
  * Collaboration: Interaction between objects and or roles
* CRC Cards: Method for thinking about possible objects or roles
  * CRC = Candidates (possible role or class), Responsibilities, Collaborators
* Tell, Don't Ask: "the calling object should describe what it wants in terms of the role that its
  neighbor plays, and let the called object decide how to make that happen."
  * Better to have a high level method that makes explicit what one object offers than a bunch of
    low-level method/getter calls
  * Try to limit "asking" to value objects, as query methods on regular objects can result in them
    leaking state
* Use tests to discover the roles your object needs to interact with
  * Depends on creating mock objects to stand in for real objects

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

## Ch. 15: Towards a Real User Interface

* Change the UI from a single label to a table with auction ID, last price, last bid
* Create a `SnipersTableModel` class extending `AbstractTableModel` to add content to a Swing
  `JTable`
* First, replace label showing status text with table showing it
  * For now, only keeps track of status text
* Next, update app to display item ID, last price, last bid, and status in table
  * First, update tests to look for table row with all required info instead of just label with
    status text
  * Must pass sniper state to all event handling methods from `SniperListener`, so create
    `SniperState` value type to prevent duplication
    * Holds item ID, price, and bid
    * `SniperStateDisplayer` will use to update the `SnipersTableModel`
  * Update `SnipersTableModel` test to make sure it stores `SniperState` when it gets it and
    notifies `JTable` that it has updated data
    * Implements `getValueAt` that `JTable` will call with `rowIndex` and `columnIndex` to get data
      for each cell of the table
  * Decide to add status to `SniperState` rather than creating different objects for winning,
    losing, won, and lost
    * Rename `SniperState` to `SniperSnapshot` to better reflect its purpose
    * Create `SniperState` enum with `JOINING`, `BIDDING`, etc.
    * Replace `sniperBidding`, `sniperWinning`, etc. with `sniperStateChanged` on `SniperListener`
    * Remove `isWinning` instance variable from `AuctionSniper` and have it store last snapshot
    * Make `SnipersTableModel` the new `SniperListener` to shorten event path
* Design had to change in this chapter
  * Not necessarily a bad thing, as writing code is usually what helps improve a design
* Can be helpful to create a `Defect` exception type to throw in places where an error can only
  happen due to a programming mistake
* Were careful to keep business logic out of the UI, as UI code becomes brittle when business logic
  is mixed in
  * Also harder to change
* Intentionally made incremental changes all the way through the system
  * e.g. First label to table with status, then table with more details
  * Avoided making huge changes to app all at once
    * Makes it easier to merge work with rest of team and avoid long-lived feature branches
  * a.k.a "keyhole surgery"
* Value your time and watch out for wasteful boilerplate
  * Could indicate an abstraction is missing
* Need to be able to balance tradeoff of whether to change design now or wait till later
  * "Deciding when to change the design requires a good sense for tradeoffs, which implies both
    sensitivity and technical maturity: “I’m about to repeat this code with minor variations, that
    seems dull and wasteful” as against “This may not be the right time to rework this, I don’t
    understand it yet.”"
  * As you're coding, reflect on if you're spending your time the best way
* Don't be afraid to rename things as the design evolves
  * "Just as we learn more about what the structure should be by using the code we've written, we
    learn more about the names we’ve chosen when we work with them. We see how the type and method
    names fit together and whether the concepts are clear, which stimulates the discovery of new
    ideas. If the name of a feature isn’t right, the only smart thing to do is change it and avoid
    countless hours of confusion for all who will read the code later."

## Ch. 16: Sniping for Multiple Items

* First, update tests and `ApplicationRunner` so it can take an auction as an argument to each of
  its methods
  * Made its `startBiddingIn` method take a variable number of auctions
* Next, update `Main.main` to take a variable number of item IDs
  * Have it look over each one and call its `joinAuction` method for it
* Then, add `addSniper` method to `SnipersTableModel` so `joinAuction` in `Main` can call it
  * Update model's test to confirm calling `addSniper` notifies the `JTable` listener
  * Update model to hold multiple `SniperSnapshot`s
  * Add `isForSameItemAs(snapshot)` to `SniperSnapshot` so table can easily tell when two snapshots
    are for the same item
  * Write test to confirm model displays snipers in the order they were added
* Next, add a text field and button to top of UI so user can join an auction with an item ID
  * Add `startBiddingFor(itemId)` method to `AuctionSniperDriver` so end-to-end test will join
    each auction via the UI
  * Test fails because UI hasn't been added yet, so add text field and button to UI
  * Now test fails because using the UI doesn't join an auction
  * Need to add an event listener to the button that gets item ID from text field, creates a chat
    connection to the auction server, and adds a row to UI table model
    * Want to keep chat connection out of UI code (`MainWindow`)
  * Create a `UserRequestListener` interface so `MainWindow` can let `Main` know when user wants
    to join a new auction
    * Have to write integration test for `MainWindow` since Swing code is async
    * `UserRequestListener` will implement `joinAuction(itemId)`
    * Add `addUserRequestListener` to `MainWindow` and update it to add an `ActionListener` to the
      button
      * When button clicked, `MainWindow` will translate the UI event into a business domain event
  * Next, update `Main` to register itself as a `UserRequestListener` that will add a new row to the
    `SnipersTableModel` when the user asks to join a new auction
    * Will also create a new `Chat` and `XMPPAuction` and hook up the chat to an
      `AuctionMessageTranslator` and an `AuctionSniper`
* Now things are looking pretty good, but `Main` has a lot of compromises
* Continuing to write tests first keeps design focus and behavior in the right objects
* Want to work on breaking up `Main` next, as it has lots of responsibilities and can only be tested
  with end-to-end tests

## Ch. 17: Teasing Apart Main

* Goal: Separate UI, chat, and sniping logic in incremental steps without breaking the app
* A top-level class like `Main` should introduce components to each other and then step into the
  background and wait for the program to finish
  * But `Main` currently implement some component logic as well as starting the program
* First, will isolate the chat logic
  * Need to separate it from the auction and sniper related code
  * Use the `Announcer` class to create a list of event listeners for the `AuctionMessageTranslator`
    to call and add the sniper to the list
    * So the translator doesn't have a direct reference to the sniper and can have multiple
      listeners
  * Then move chat creation into `XMPPAuction`
    * In its constructor will create chat from passed connection and new translator connected to the
      chat and the announcer
      * Ideally, would prefer the constructur to have fewer assumptions and just set fields w/o
        causing side effects, but will keep it for now
    * The `Main` can just pass the new `AuctionSniper` to the auction's `addAuctionEventListener`,
      which will proxy it to the translator
    * Write integration test to confirm `XMPPAuction` can listen to the chat
    * Now that `XMPPAuction` encapsulates creation and interaction with the chat, put it and
      `AuctionMessageTranslator` into `auctionsniper.xmpp` package
  * Next, create an Auction House as a factory for creating `Auction`s
    * Create `AuctionHouse` interface and `XMPPAuctionHouse` class
    * Then can move the XMPP connection code out of `Main`
    * Will implement `connect(host, username, password)` for connecting to chat server
    * Will implement `auctionFor(itemId)` to be called when UI requests to join an auction
* Now the UI code
  * Next, create the `SniperLauncher` class to replace the anonymous implementation of
    `UserRequestListener` in `Main`
    * Will take care of getting auctions from the `AuctionHouse` and managing the
      `SnipersTableModel`, provided they are passed to its constructor
    * Implements `joinAuction` for the UI to call
      * Will handle creating new `AuctionSniper`s and connecting them to the UI's table model via
        `SwingThreadSniperListener`
        * But want to get the Swing-related code out of there
  * Next, create a `SniperCollector` interface and update the `SnipersTableModel` to implement it
    * `SniperLauncher` will pass it `AuctionSniper`s and it will deal with connecting them to the UI
    * Will get `SniperSnapshot` from the sniper and notify the `JTable` to add it to the UI
    * Will hook up the sniper to the `SwingThreadSniperListener`
    * This gets UI logic out of the launcher
  * Next, create `SniperPortfolio` to hold all the snipers
    * So the table model doesn't have to be responsible for both keep track of the sniping activity
      and updating the UI
    * Move implementation of `SniperCollector` from table model to the portfolio
    * Update `MainWindow` to be instantiated with a `SniperPortfolio`
    * Create `PortfolioListener` interface with `sniperAdded` method and have `SnipersTableModel`
      implement it
      * `MainWindow` now instantiates the table model and passes it to the portfolio via its
        `addPortfolioListener` method
  * Now all `Main` does it create the portfolio and the `MainWindow`, and add the `SniperLauncher`
    as the main window's UI request listener
* Now the app is separated into three components:
  * The core
  * XMPP communication
  * The Swing UI
* Worked to keep external infrastructure details out of core domain code
* Made sure to work incrementally
  * "By repeatedly fixing local problems in the code, we find we can explore the design safely,
    never straying more than a few minutes from working code. Usually this is enough to lead us
    towards a better design, and we can always backtrack and take another path if it doesn’t work
    out."

## Ch. 18: Filling In the Details

* Add ability to stop bidding once a stop price is reached
* Have followed agile pattern: focus on proving concept and get support to continue, then implement
  enough functionality to deploy
* Create new state for when sniper has been outbid
  * It'll stay in auction so user can know how much they lost by
* Start with an acceptance test
  * Add `startBiddingWithStopPrice` and `hasShownSniperIsLosing` to the `ApplicationRunner`
* Add text field to UI to accept stop price
* Create an `Item` class to hold the item ID and stop price from the UI
  * Better than adding a new stop price arguments to all methods in the chain
  * Is example of "budding off"
  * Good to main domain concepts explicit
* Then update `AuctionSniper` to stop bidding at stop price
  * Calls `allowsBid(bid)` on `Item` to see if it should continue bidding or stop
* Drew a state transition diagram to help figure out what tests to write

## Ch. 19: Handling Failure

* Have app write to a log file if something goes wrong
* If Auction sends back malformed message, will show it as "failed" in UI and ignore further
  messages
* Create end-to-end test
  * Add `sendInvalidMessageContaining` to fake auction server and `showsSniperHasFailed` to
    `ApplicationRunner`
* Add `auctionFailed` method to `AuctionEventListener` interface
  * `AuctionMessageTranslator` will call it if it can't parse the message
* Also mark as failed if message is missing a required field
* Remove sniper's translator from chat's message listeners to ignore further updates
  * Create an `AuctionEventListener` implementation that will do this when it receives
    `auctionFailed` call
    * Could have done this in the translator, but that wouldn't respect SRP
* Next, write failing test for recording message to log file
  * Create `AuctionLogDriver` to read log file from disk and make assertions about entries it has
* Create `XMPPFailureReporter` interface for object that logs message and pass it as dependency to
  translator
  * Translator can call its `cannotTranslateMessage` method
* Then create `LoggingXMPPFailureReporter` implementation of `XMPPFailureReporter`
  * Will actually write to file
  * Pass it to `XMPPAuctionHouse` so it can pass it to every translator
* Important concepts
  * Add functionality in small, coherent slices
  * Keep code well-structured so it can be extended
  * Refactor frequently to make space for new functionality
  * Treat production logging like the external interface it is
    * Is an interface like the UI
    * Should be properly abstracted; don't put ad hoc logging calls carelessly throughout code
    * Logging infrastructure and implementation details should be isolated
    * Develop logging against a stakeholder's requirements using TDD
    * Ideally, logging is an implementation detail that the rest of your code can ignore when it
      notifies your logging infrastructure that something has occurred
      * Write logger interface in terms of the app domain

## Ch. 20: Listening to the Tests

* TDD gives you feedback on internal quality of code
  * On coupling, cohesion, dependencies, information hiding
* Make dependencies on time explicit
  * e.g. pass in a date instead of creating it inside class
* Strive to make boundaries of an object clearly visible
  * "An object should only deal with values and instances that are either local—created and managed
    within its scope—or passed in explicitly,"
  * Then object is context-independent and can be easily moved and reused
* Don't use magic testing tools to allow for poorly managed dependencies
* Strive to only mock interfaces, not concrete classes
  * An interface describes a role an object plays and its relationship to the consuming object, so
    mocking the interface in tests maks this relationship explicit
  * Naming interfaces helps you think in detail about a concept
* Prefer to write a facade in front of an external library and then mock the facade instead of the
  library
* Don't bother mocking value objects; just use them directly
* Watch out for bloated constructors that take too many arguments
  * Maybe some of the arguments can be combined into one object that represents an overall concept
    * Look for things that are always used together
  * Dependencies should always be passed into the constructor, but notifications and adjustments can
    be set to defaults and reconfigured later
    * Adjustments = Parts of a composite object
      * e.g. A `RacingCar`'s tires are adjustments, while a track is a dependency
      * Should be able to replace them in unit tests
* Keep number of expectations to a minimum to keep it clear what's important
  * Just stub out a call to an object that's unimportant to the test instead of setting an
    expectation on it

## Ch. 21: Test Readability

* Tests should concretely describe what code does but be abstract about how it works
* "name tests in terms of the features that the target object provides"
  * "the test name should say something about the expected result, the action on the object, and the
    motivation for the scenario."
* Write tests using a standard structure so they're easy to skim
* One test should exercise one coherent feature
  * Not limited to one assertion but shouldn't be more than a handful
* Keep tests as simple as possible by moving as many implementation details as possible into helper
  methods or objects
  * A test should just contain what's necessary to understand it at a high level
* Don't catch an exception unless you're going to assert something about it
* Tests shouldn't assert too much or too little detail
  * "The assertions and expectations of a test should communicate precisely what matters in the
    behavior of the target code."
* Use descriptive constants and variables to describe the meanings of literal values

## Ch. 22: Constructing Complex Test Data

* Object creation code can clutter tests and make them hard to read
  * Also break more easily when object construction details change
* Better to abstract object creation into helper objects
  * **Object Mother:** Supplies descriptively-named factory methods for constructing objects
  * **Test Data Builders:** Objects that use the *builder pattern* to construct objects for tests
    * Will have fields and setters for each constructor argument the object it's creating takes
    * Should provide default values for object's constructor arguments that tests can override
    * You can wrap creation of the builders themselves in factory methods to keep their creation
      details out of the tests
    * They make it easy to support lots of variations in constructor arguments
* Aim for tests that are declarative descriptions of code features

## Ch. 23: Test Diagnostics

* Test assertions should provide detailed failure messages so a dev doesn't have to spend time
  debugging why a test failed
* First, keep tests small and give them descriptive names
* If assertion library lets you pass a failure message with an assertion, do it
* You can make primitive values descriptive of what they're used for in the tests
  * e.g. `AN_INVALID_STRING = "invalid string"`
* **Tracer Objects** are objects that have no behavior except to produce descriptive error messages
* Before making failing tests pass, make sure the failure messages are clear and informative

## Ch. 24: Test Flexibility

* A test should only fail when the code relevant to it is broken
* A test should "specify precisely what should happen and no more"
  * "Avoid asserting values that aren't driven by the test inputs, and avoid reasserting behavior
    that is covered in other tests."
  * If only one piece of a result matters, test just that piece instead of comparing the entire
    result
    * "Different test scenarios may make the tested code return results that differ only in specific
      attributes, so comparing the entire result each time is misleading and introduces an implicit
      dependency"
    * e.g. Check that a certain value is included in a resulting string instead of asserting the
      entire format of the string
      * Though if there's a lot of data in a string, it could be an indication you're missing a
        value object to structure the data
* Allowances vs. expectations
  * Allowances allow a mock object to optionally receive a call but don't enforce it
    * Use to ignore mock calls that aren't important to the behavior currently being tested
      * But make sure ignored features are tested somewhere
    * Can be used to feed values into tested object
  * Expectations expect that a mock object will receive a call
    * "Expectations describe the interactions that are essential to the protocol we're testing"
* "Allow queries; Expect commands"
  * "Commands are calls that are likely to have side effects, to change the world outside the target
    object."
  * "Queries don’t change the world, so they can be called any number of times, including none."
* Don't set expectations on the order of mock method calls unless order really matters
* **Guinea Pig Object:** A fake object that stands in for an application domain object during a test
  * Use them so your tests aren't coupled to real domain objects
  * They can written in a way that's much more descriptive
