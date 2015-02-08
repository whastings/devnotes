# Development Best Practices

Here are some best practices that I've found particularly noteworthy and so
have listed here for my reference. There are, of course, many more, so I've
provided links to sources of further best practices where applicable.

## Coding in General
- Limit source files to 300 lines max.
- Limit methods to 10 lines.
- *"Make names as long as necessary, but as short as possible."*
- A method should only do one task that you can describe in one sentence.
    + The method's name should describe all that it does.
- Add line breaks between logical portions of a method ("paragraphs").
- A method shouldn't modify the state of its arguments unless that's the explicit purpose of the method.
- Avoid nesting code (loops, blocks, conditionals) more than two levels.
    + When it happens, break nested code into helper methods.
- Avoid single-character variable names.
- Limit arrays to one type of object.
- Don't code for features that may or may not become requirements in the future or try to generalize code for every possible situation.
    + Follow **YAGNI** (*You ain't gonna need it*)
- Keep classes below 125 lines if possible and definitely below 300.
- **References**
    + [Thoughtbot's Best Practices](https://github.com/thoughtbot/guides/tree/master/best-practices)
    + [Thoughtbot's Style Guide](https://github.com/thoughtbot/guides/tree/master/style)

## JavaScript
- Don't declare functions inside loops or conditionals.
- Take advantage of `"use strict";`
- Always prefer `===` and `!==` over `==` and `!=`.
- Use a variable called `self` to alias `this` when you need to refer to the `this` value for an outer scope.
- Cache an array's length when iterating over it in a `for` loop.
    + e.g. `for (var i = 0, l = array.length; i < l; i++)`
- Use curly braces even for one line conditionals and loops.
- Strive to declare local variables at/near the top of their function.
- In HTML, use a common prefix for classes and IDs that are for JS usage.
    + e.g. `.js-toggle-button`, `#js-user-form`
    + Also, don't use them for CSS styling
- When using jQuery, prefix all variables that hold jQuery objects with `$`.
    + e.g. `var $toggleButtons = $('.js-toggle-button');`
- Always assign global variables to the global object instead of creating them implicitly.
    + `window` in the browser. `global` in Node.
- **References**
    + [45 Useful JavaScript Tips, Tricks and Best Practices](http://flippinawesome.org/2013/12/23/45-useful-javascript-tips-tricks-and-best-practices/)
    + [AirBNB's Style Guide](https://github.com/airbnb/javascript)
    + [Crockford's Style Guide](http://javascript.crockford.com/code.html)
    + [JavaScript Patterns Repo](https://github.com/shichuan/javascript-patterns)
    + [jQuery's Style Guide](http://contribute.jquery.org/style-guide/js/)

## Learning
- *Always* ask questions.
- Step out of your comfort zone.
- Strive to understand everything *deeply*.

## Pair Programming
- Talk about approach to the problem before starting it.
- As navigator, clearly describe your intentions to the driver.
- As driver, listen carefully, don't go ahead of the driver's instructions, and ask questions whenever something isn't clear.
- Switch roles frequently.

## Rails
- Use FactoryGirl's `build_stubbed` method to speed up specs for validations and instance methods.
- Write specs to test database constraints by saving models without running validations.

## Ruby
- Name methods without side effects for what they return.
    + e.g. `greeting` instead of `get_greeting`
- Name methods with side effects with verbs (e.g. `print_state`).
- Prefer **string interpolation** over concatenation.
- Use **parallel assignment** to swap variable values.
- Use an underscore to denote unused block variables.
- Avoid returning from within blocks except for basic control flow (e.g. a method that searches a collection with `each` and returns when value is found).
- When writing a method with a bang and non-bang version (e.g. bubble_sort and bubble_sort!), a good pattern is to implement logic in bang-version and have non-bang version dup self and call the bang version on the duplicate.
- **References**
    + [The Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide)
    + [GitHub's Ruby Style Guide](https://github.com/styleguide/ruby)
    + [Thoughtbot's Style Guide](https://github.com/thoughtbot/guides/tree/master/style#ruby)

## SQL
- For databases that support it, use a constraint to ensure that an row a foreign key refers to exists in the foreign table before the referencing row can be saved.
- Break queries into multiple lines for readability. For example:
```
SELECT
  col_1,
  col_2
FROM
  table_name
WHERE
  condition
```
- **References**
    + ["How I Write SQL"](http://www.craigkerstiens.com/2012/11/17/how-i-write-sql)
