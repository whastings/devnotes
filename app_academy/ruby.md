# Ruby & Programming Curriculum Notes - Will Hastings

## Week 1 - Day 1
**DOOR CODE**: #6435
Have questions for lecture time on previous day's work and readings
**Pair Programming**
- Person at controls (driver) does the typing, while the observer (navigator) communicates what to do next in plain English.
    + Driver should ask questions frequently about navigator's directions
- Pair should switch roles frequently (every 15 min)
**Weekly Assessments** on Mondays
- Will be a practice assessment first.
Weeks 1 - 7 are learning material
Weeks 8 - 9 are final projects
Weeks 10 - 12 are algorithms, practice problems, job searching
Email **daily progress report** to your TA

## Week 1 - Day 2
Way to round down to a ten digit: `(two_digit_num / 10) * 10`
*object*.**tap** allows you to pass an object into a block and returns the object after the block
- Useful if you're creating a new object without saving a reference to it to a variable
Integers (Fixnums) are immutable objects that are created at the beginning of the program
- Integer literals always refer to the same object
**Syntactic Sugar** examples: `variable += value`, `variable != value`, `array[index] = value`
Prefer calling a getter/setter method in a class to reading from/assigning to the instance variable
- Particularly, never call `setter_method = value` in a method, as it will create a variable instead of calling the method
    + You'll need to do "*self*.setter_method = value"
You can check if a number is a factor of another number with `number % possible_factor == 0`

## Week 1 - Day 3
**Code Smells**
- Duplicated/similar code (not *DRY*)
- Methods that are too long (> 10 lines)
- Calling methods on objects internal to another object
    + Violates the **Law of Demeter** (*Only talk to your neighbors*)
- Exposing (keeping publics) methods that aren't absolutey necessary for a class's users to access
- *God Objects* that know too much about lots of other objects
Be careful when setting a default value for array elements or hash defaults
- Can do with:
    + `Array.new(num_elements, default_value)`
    + `Hash.new(default_value)`
- But if you use a *mutable object*, such as a new array or string, the *same object* will be used for every element
    + If you want to use such an object, use the block constructor version
        * e.g. `Array.new(num_elements) { [] } # New array for each element`
**Array Destructuring**
- `x, y = array` will assign the first to elements of array to x and y
- `x, *y = array` will assign first element of array to x and the result of elements as array to y
To call a class method from an instance method, use `self.class.class_method` instead of `ClassName.class_method`
- Don't have to worry if renaming the class
- Allows `class_method` to be redefined by subclasses
You can use `retry` in `begin...end` to repeat its body based on a condition

## Week 1 - Day 4
**Explicit Block Parameter** 
- If you have an explicit block parameter (with an `&`), it will be `nil` if no block is provided
- Using it instead of yield makes it easier to see that your method takes a block, but at the cost of a little performance
- QUESTION: Should I worry about the performance impact of using explicit block parameters
    + Answer: Not really. Better readability is more important.
QUESTION: Is returning in a block okay if used for control flow?
- e.g. Iterating with each till you find a desired value, then returning it
- Answer: It's okay, but it's about the only case
`Symbol#to_proc`
Behavioral differences between Procs and Lambdas
- Lambda enforces the number of arguments passed to it, while Procs set unpassed methods to nil
- `return` in a Proc returns from the method the Proc was defined in, while `return` in a Lambda simply exits the Lambda
- "Procs have *block semantics*. Lambdas have *method semantics*."
The **ampersand** operator "blockifies a proc" and "procifies a block", depending on if it's used in a paramter list or a method call's argument list
**Recursion**:
- **Base Case**: The condition at which a recursive method stops recursing and returns back up the call stack
- *Recursive programming strategy*:
    + Figure out what the base case is
    + Find out how to break the problem into smaller problems, aiming for the base case
    + Figure out what happens in your method at one level before the base case.
        * Does this generalize to all other levels?
    + Figure out what type of value your method returns and make sure you return that type of value in all situations
- **Tail Recursion**: Having the recursive call at the bottom of the recursive method
    + Ruby doesn't optimize for this
Paired with Alex

## Week 1 - Day 5
Two cool methods for getting subarrays
- `Array#take` takes a number and returns the first *number* elements from the array
- `Array#drop` takes a number and returns array with the first *number* elements dropped
The **Set** class is much faster when running `include?` than an array.
- Can initialize a set with `Set.new(an_array)`
- Set maintains a hash of its elements, using a search similar to binary search
- But array has to look through each element till it finds it (linear time)
QUESTION: How does your exponent2 solution work without using the different formulas for even and odd numbers?
- Even equation: 2**2n = 2**n * 2**n
- Odd equation: 2**2n+1 = 2**n * 2**n+1
- Can solve both by doing: (2**n/2).floor * (2**n/2).ceil
Recursion tends to use more memory than iteration and take slightly longer
The **&& operator** returns the last falsey value if there's one, otherwise it returns the last value
- So you can use it for control flow
    + e.g. `some_condition && do_something`
Time yourself when doing practice assessment
**Tree Data Structure** 
- Bottom nodes with no children are **leaves** 
- There's always a **root node**
- Each **node** has a parent, children, and a value
**Depth First Search**: Tree searching algorithm which goes down through nodes to leaves then backs up.
**Breadth First Search**: Tree searching algorithm which goes through each level of nodes in the tree horizontally before moving downward
- Tends to find a more direct path to a solution than DFS
- But is harder to do with recursion
- Easier to use a queue: Start with root node, then pull it out of queue and add its children
    + Repeat for children of all nodes until target found or no more nodes to search
**Binary Tree**: Tree in which each parent has at most two children
- You can consider each node and its children a *subtree*
Pair with Allen

## Week 1 - Days 6 - 7
Version Control: System for storing changes to a project so they can be tracked, restored, etc.
**Distributed Version Control System** (DVCS): A VCS in which each user retains an entire copy of the repository's history
Git stores data in snapshots as opposed to differences
- Difference: A record of only the changes between one version of a file and the next
- Snapshot: A record of an entire project at a specific point in time
**File states in Git:**
- Committed: Git is tracking the file and it has not been changed since the last snapshot
- Modified: Git is tracking the file and it has been changed since the last snapshot
- Staged: Git is tracking the file, it has been modified, and you have marked it as "staged" so its changes will be part of the next commit.
**Three Areas of a Git Repo:**
- .git directory, which holds object database/version history.
- Working directory, which holds the latest version of the repo plus any changes you've made to it.
- Staging Area: Which holds the changes you've marked to go into the next commit.
`git diff` shows unstaged file changes
`git diff --cached` shows staged file changes
Use `git add -u` to stage only updated or deleted tracked files, not untracked files.
Use `git mv` to move or rename tracked files.
Use `git rm` to delete and stage deletion of files.
Git keeps track of what branch you're currently on by pointing `HEAD` to the branch.
A **branch** is simply a pointer to a particular commit.
A **remote branch** is a pointer to the commit a given branch is on from a certain remote
- You can't move it (without pushing), but you can see it.

## Week 2 - Day 1
When writing a method with a bang and non-bang version (e.g. bubble_sort and bubble_sort!), a good pattern is to implement logic in bang-version and have non-bang version dup self and call the bang version on the duplicate.
**Using JSON**
- Load JSON library with `require "json"`
- To serialize object to JSON, call `object.to_json`
    + Can only do basic types like arrays, hashes, and strings out of the box
        * To do a custom class, you'd have to define a to_json method in the class
- To parse a JSON string, use `JSON.parse(json_string)`
**Using YAML** 
- Load YAML library with `require "yaml"`
- To serialize object to YAML, call `object.to_yaml`
    + Can serialize custom classes with simple type instance variables out of the box
    + Stores which class the serialized object was from
- To parse a YAML string, use `YAML.load(yaml_string)`
Minesweeper
- Don't store number of bombs as a property
Partner: Weyman Fung

## Week 2 - Day 2
The **retry** keyword allows you to repeat a `begin...end` block when an exception occurs
- Must be called in a `rescue` clause
An `ensure` clause will run *after* any `rescue` clauses
You can catch any exception type with `rescue => error_var`
**private** semantics: Method can only be called on an instance by that instance from inside its class
- Can't be called with an explicit receiver, even `self`
**protected** semantics: Method can only be called on an instance by that instance or another instance of the same class
In an overridden method, calling `super` without any arguments will automatically pass on any arguments to the superclass's method
Instance variables defined in a superclass are available in a base class.
`Integer(string)` will throw an exception if it can't convert string to an integer
- string.to_i will just return 0 for any string it can't understand
**Chess**
Classes:
- Board
- Piece
    + Sliding Piece
        * Rook
        * Bishop
        * Queen
    + Stepping Piece
        * Knight
        * King
    + Pawn
- Player
    + HumanPlayer
    + ComputerPlayer
- Game
Piece vars: Color, position, ref to board

## Week 2 - Day 4
If you want to define a method is a superclass just to show it needs to be implemented in a subclass, you can have it throw at `NotImplementedError` if called.
- Kind of like an abstract method from Java
You can keep an abstract base class from being instantiated by raising an error in its initialize if `self.class == ClassName`, where `ClassName` is the abstract base class's name
Calling `raise string` will raise a `RuntimeError` with that string as the message
**Checkers** 
- Hard part is validating a multi-jump move
    + Should use a duped board to perform all moves to make sure they're valid before calling on original board

## Week 2 - Day 5
### RSpec
Tests make *good documentation* for code
Tests eleveate fear of refactoring
Only test code's *public interface* (not private methods)
Equality Expectations
`expect(var).to eq(other_var)` tests `var == other_var`
`expect(var).to be(other_var)` tests if they are the same object
`expect(var).to eql(other_var)` tests `var.eql?(other_var)`
File organization:
- Put tests in a `spec` directory and put code in a `lib` directory
    + RSpec automatically adds lib to the load path
Group expectations with `it` blocks
Group `it` blocks with `describe` and `context` blocks
Use `expect` to test expectations
- `should`  is deprecated
RSpec comes with easy Rake integration for setting up a spec running task
```
require 'rspec'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec) do |t|
  t.fail_on_error = false
  t.rspec_opts = ["--format documentation", "--color", "--order=default"]
  t.pattern = ARGV[1] || "spec/*_spec.rb"
end

task :default => :spec
```
**Red, Green, Refactor Workflow**
- Red: Write tests first and make sure they fail
    + Need to make sure tests actually fail with the implementation code they're supposed to be describing
- Green: Write implementation code to get the tests to pass
- Refactor: Refactor code as needed, using the tests to ensure you don't break it
**Doubles/Mocks**
- Should be used in *unit* tests
- Should not be used in *integration tests*, as those need to test actual interactions between real objects
The `let` keyword does not maintain state between `it` blocks
Can store rspec options for your project in a `.rspec` file
Running `rspec --init` in your project directory will generate a `spec` directory with a `spec_helper.rb` file for configuring rspec and a `.rspec` file for cli options
**Collection Membership Test**: RSpec can test that a collection should `include` an element, `start_with` or `end_with` one or more elements, and `contain_exactly` an element a number of times
You can use **Compound Matcher Expressions** to combine expectations with && or || logic
- e.g. `expect(var).to start_with('a').and end_with('z')`
- e.g. `expect(stoplight.color).to eq("red").or eq("green").or eq("yellow")`
