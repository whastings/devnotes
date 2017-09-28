# Book Notes: Practical Object-Oriented Design in Ruby

## Ch. 1: Object-Oriented Design

* "[the book] views the world as a series of spontaneous interactions between objects"
* Think about programming as messages passing between objects
* The purpose of code design is to support the need for code to change
  * If your code will never be changed, design doesn't matter
* Dependencies between objects make change difficult
  * But there must always be some dependencies between objects, as for one object to send a message
    to another, it must know something about it and therefore have a dependency on it
  * OO design helps you manage dependencies so code is easier to change
* Don't design to try to anticipate future requirements
  * "Designs that anticipate specific future requirements almost always end badly."
  * Change will happen, but you can't know now what it will be
* Patterns and principles are a code designer's tools
* Design should be done while coding
  * "Design is a process of progressive discovery that relies on a feedback loop."
* "Agile believes that the most cost-effective way to produce what customers really want is to
  collaborate with them, building software one small bit at a time such that each delivered bit has
  the opportunity to alter ideas about the next. The Agile experience is that this collaboration
  produces software that differs from what was initially imagined; the resulting software could not
  have been anticipated by any other means."
  * So "Big Up Front Design" doesn't work
  * You can't know when program will be done
* OO languages allow you to invent brand new types of your own

## Ch. 2: Designing Classes with a Single Responsibility

* Messages are the foundation of an OO system, but classes are more visible
* Goal: model your app using classes so it satisfies today's requirements and can be easily changed
  for future requirements
* Code should be:
  * Transparent: Consequences of changes should be obvious
  * Reasonable: Cost of a change should be proportional to its benefits
  * Usable: Should be usable in new and unexpected consequences
  * Exemplary: Should encourage others to perpetuate these qualities
* Classes that have multiple responsibilities are difficult to reuse
  * You can't just use the parts you need w/o getting the parts you don't need
* Heuristic: If describing class in a sentence contains "and", you probably have multiple
  responsibilities
* Cohesion: How related everything in a class is to its central purpose
* When to make design decisions
  * Postpone them as long as possible so you can get more info
  * If cost of making decision later will be the same as making it now, wait
  * "This “improve it now” versus “improve it later” tension always exists. Applications are never
    perfectly designed. Every choice has a price. A good designer understands this tension and
    minimizes costs by making informed tradeoffs between the needs of the present and the
    possibilities of the future."
* Always access instance variables via methods
  * Even within the same class
  * So if something has to change about it, can do it in one place instead of many
* If your class depends on a complex data structure (e.g. array of arrays) hide the structure
  * e.g. Using a struct that defines methods to access data
* Methods should have single responsibility too
  * e.g. Separate iterating over structure from operation applied to each element
  * Small methods are easier to reuse and easier to move into other classes

## Ch. 3: Managing Dependencies

* "An object depends on another object if, when one object changes, the other might be forced to
  change in turn."
* An object has a dependency when:
  * It knows the name of another class
  * It knows the name of another object's method
  * It knows the arguments another object's method requires
* If an object has dependencies, you can't reuse it without them
* Tests can also be over-coupled to objects
* When you hard-code a class name that an object depends on, you're making it impossible for it to
  collaborate with other types of objects even if they implement the same interface
* Prefer to pass in an object's dependencies so it doesn't need to know their types or how to create
  them
  * This is **dependency injection**
* If you can't pass in a dependency, at least isolate its creation in an explicit place so the
  existence of the dependency is clear
  * e.g. In the object's constructor or in a method dedicated to instantiating that dependency
* Also isolate messages sent to objects other than `self`
  * e.g. If you need to send `wheel.diameter`, only do it in a `diameter` method
  * Then you can only update it in one place if you need to change something about how you call it
    or use its return value
* If you can't change something that's complex to use (e.g. lots of positional arguments), wrap it
  in your own wrapper than can hide the complexity from the rest of your code
* Choose your dependency direction by having the thing more likely to change depend on the thing
  less likely to change
  * Try to figure out who's most likely to change
  * Abstractions usually change less often than concretions
* The more dependents a class has and the more likely it is to change, the more trouble it will be
  * If a class is unlikely to change, having many dependents isn't that bad
  * If a class has few dependents, having to change it isn't that bad
  * Watch out for concrete classes with many dependents

## Ch. 4: Creating Flexible Interfaces

* Design is concerned with how objects communicate via messages
* Interface = an object's public methods that other objects can use to send it messages
* Public methods:
  * Shouldn't change on a whim
  * Should be documented via tests
  * Describe the responsibilities of a class
* Don't put too much focus on the objects that make up your app's domain
  * Instead, focus on the messages that pass between them, which will help you discover new, less
    obvious objects
  * Don't fixate on them or you'll end up trying to coerce them into having the behavior you need
* Use **Sequence Diagrams** to cheaply experiment with objects and messages
  * Use them to clarify your thoughts on your design before you start coding
  * Make the messages passing between objects explicit
  * Helps you focus on messages instead of classes
    * You discover messages first and then figure out where to send them instead of creating classes
      and then trying to figure out what methods they should support
  * "This transition from class-based design to message-based design is a turning point in your
    design career. The message-based perspective yields more flexible applications than does the
    class-based perspective. Changing the fundamental design question from “I know I need this
    class, what should it do?” to “I need to send this message, who should respond to it?” is the
    first step in that direction."
  * When an object messages another object, it should "ask for 'what' instead of telling 'how'"
  * **Context:** What an object knows about other objects, and what it expects from its environment
    * The simpler an object's context and the less it expects, the easier it is to reuse and test
    * e.g. A `Trip` class that calls `prepare_bicycle` on a `Mechanic` for each bicycle it has, vs.
      a `Trip` that calls `prepare_trip` on a `Mechanic` with itself, and then lets the `Mechanic`
      call `bicycles` on it
      * `Trip` doesn't know what `Mechanic` is or what it does
    * Move from writing procedural code to code where objects trust each other
      * Let's objects collaborate with requiring complex context
* Prefer methods that take a hash of options instead of positional arguments
* Create interfaces that require little context from callers and don't require callers to know how
  they implement their behaviors
* Law of Demeter violations usually indicate an interface that is missing a message

## Ch. 5: Reducing Costs with Duck Types

* "Duck types are public interfaces that are not tied to any specific class. These across-class
  interfaces add enormous flexibility to your application by replacing costly dependencies on class
  with more forgiving dependencies on messages."
* Allow objects to care about what other objects do, not what they are
* You have to keep an eye out for duck types and design their interfaces as intentionally as those
  of classes
* e.g. A `Trip` could work with objects like `TripCoordinator` and `Driver` that respond to a
  `prepare_trip(trip)` method
  * They implement the `Preparer` duck type
* Concrete code is easy to understand but hard to extend, while abstract code is harder to
  understand but easy to extend
  * Duck types are abstractions
* Finding duck types
  * Looks for code that checks `class`, `is_a?` calls, and `responds_to?` calls

## Ch. 6: Acquiring Behavior Through Inheritance

* Inheritance is a mechanism for automatic message delegation
  * If object's class doesn't understand the message, it'll forward to message to its superclass
* Example: `Bicycle` class that's instantiated with `size` and `tape_color`
  * Has `spares` method that returns hash of `chain` (e.g. `10-speed`), `tire_size`, and
    `tape_color`
  * This works for road bikes, but what if we now have to support mountain bikes?
  * Mountain bike needs to know about its `front_shock` and `rear_shock`
  * Could add a `style` instance variable to `Bicycle` so it can support both road and mountain
    bikes
    * But this won't scale if we have to support more bike types in the future
    * And some instances will respond to `tape_color` or `front_shock` and others won't
  * `Bicycle` has some code that applies to all bikes, some that just applies to road bikes, and
    some that just applies to mountain bikes
    * "This is the exact problem that inheritance solves; that of highly related types that share
      common behavior but differ along some dimension."
* When there's a conditional determining what message to send to `self`, inheritance may be in order
* Watch out for variables used to describe the type or category of an object
* A subclass delegates an unknown message to its superclass
  * So a subclass is everything its superclass is plus more (a specialization)
* First attempt: Create a `MountainBike` subclass of `Bicycle`
  * Implements `initialize` to handle `front_shock` and `rear_shock`, passing rest of init data to
    `super`
  * Implements `spares` to get default hash from `super` and merge in `rear_shock`
    * But now `MountainBike` still reports needing handlebar tape, as `Bicycle` really represents a
      road bike
* Next, move road bike specific logic into a `RoadBike` subclass of `Bicycle`
  * Now `Bicycle` can be an abstract class that's a "common repository of behavior" for subclasses
    to share
  * Better to wait to create an abstract class until you have at least two, preferably three,
    subclasses
    * May be better to duplicate code until you know more
* When creating an abstract class from an existing one, it's best to create an empty abstract class
  and selectively "promote" bits of behavior from the previous class to the abstract one
  * So rename `Bicycle` to `RoadBike` and create an empty `Bicycle` to be abstract
  * If you try to pull down just parts relevant to concrete class, you may accidentally leave
    concrete details in the superclass
  * If you forget to promote something, it'll become apparent when another subclass needs it
* When making a decision, ask "what will happen if I'm wrong?" and look for the decision where the
  answer is the best
* Next, create accessors in `Bicycle` for `size`, `chain`, and `tire_size`, which are common to all
  bikes
* Template Method pattern:
  * Involves the superclass sending messages to subclasses to give them a chance to contribute
    specializations
  * e.g. `Bicycle#initialize` can call `default_chain` and `default_tire_size` to get the default
    values of these from its subclasses
  * Superclass should implement all messages that it sends to subclasses, raising a
    `NotImplementedError` for ones that subclasses must override
  * Use it to avoid using `super`
    * If you have a method that must send `super`, someone will forget to call it
    * Also it causes subclasses to know too much about their superclass and be too tightly coupled
      to them
    * The super class should instead send *hook messages* to subclasses
    * e.g. `Bicycle` can send `post_initialize` and `RoadBike` and `MountainBike` can implement it
      instead of defining `initialize` with `super`
    * e.g. `Bicycle#spares` sends `local_spares` and merges result into hash
  * How the template method is used can change in superclass with requiring subclasses to change

## Ch. 7: Sharing Role Behavior with Modules

* "Because no design technique is free, creating the most cost-effective application requires making
  informed tradeoffs between the relative costs and likely benefits of alternatives."
* Sometimes otherwise unrelated objects play the same role
  * And sometimes they need to share behavior
  * e.g. The `Preparer` role for objects that prepare for `Trip`s (`Mechanic`, `TripCoordinator`,
    `Driver`)
    * The `Trip` can also be considered to play the `Preparable` role
* Ruby provides modules for sharing role behavior between unrelated objects
  * Including a module in an object makes it possible for the object to automatically delegate
    unknown method calls to it
* Example: A trip `Schedule` class that implements `scheduled?`, `add`, `remove`, and `schedulable?`
  * It can take `Schedulable` objects that respond to a `lead_days` method
    * `lead_days` returns how many days in between trips for that type of object
  * Even better: Have `Schedulable` objects implement `schedulable?` and deal with the `Schedule`
    themselves
    * Then create a `Schedulable` module to define `schedulable?`
* Like with abstract classes, a module should provide an implementation of every message it sends to
  `self`
  * Even if that's just raising `NotImplementedError`
* Modules can also use the Template Method pattern to allow including classes to supply
  specializations w/o knowing too much
* Modules are placed in the method lookup path in reverse order of inclusion
* Modules make it possible to write crazy code that's impossible to extend
* Methods in a module should apply to all objects that include it
  * Not just some of the methods for some objects
  * Same goes for abstract classes
* Keep your inheritance hierarchies shallow
  * Template methods don't work across more than one level
