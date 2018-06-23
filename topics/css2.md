# CSS Notes

## Box Model

* Collapsing Margins
  * Top and bottom margins between two sibling block elements will collapse
    * Unless one or both create a new block formatting context (e.g. from being floated)
  * Top margin of element and top margin of first child will collapse
    * Unless you put something between them like a border
    * Bottom will too, but only if parent is `height: auto`
  * Doesn't happen with flex elements

## Flexbox

* `flex-basis`:
  * Defines size of item before growing or shrinking
  * Starts as `auto` and sizes item based on its `width` or the size of its content
  * Can be set to zero to make all space in item available for distribution
* `flex-grow`:
  * Defines how much free space is distributed to an item
  * Is a ratio
  * If all items have same value, space will be distributed equally
    * If some items have a bigger `flex-basis` than others, they will end up bigger
* `flex-shrink`:
  * Determines how much an item will shrink relative to other items when container is too small
  * Works a bit differently than grow
    * Is multiplied by flex basis of item so small items won't shrink too much compared to big items
  * Won't shrink an item past its `min-content` size
* Wrapping
  * When allowed to wrap, each new row or column behaves like a new flex container
    * An item won't necessarily be same size as one directly above it

## CSS Grid

* Columns and rows are called **tracks**
* Turn on CSS grid with `display: grid` on a container
* Use `grid-template-columns` to define the columns of a grid container
  e.g. `grid-template-columns: 100px 100px 100px;` will give you 3 columns, each 100px wide
* Use `grid-template-rows` to do the same but for row tracks
* Use `grid-gap` to define space between items
* `repeat()` function lets you repeat a definition for N columns
  * e.g. `grid-template-rows: repeat(5, 100px)` would give you 5 columns, each 100px wide

### Explicit Tracks

* Tracks that you've defined explicitly
  * e.g. `grid-template-columns: 200px 400px;` gives you two explicit column tracks

### Implicit Tracks

* Tracks that the CSS Grid algorithm defines for you
  * e.g. If you define `grid-template-column` but not `grid-template-rows`, all the rows will be implicit
  * e.g. If you have `grid-template-columns: 200px 400px;` and `grid-template-rows: 50px 100px;` but six grid items, the last row will be sized implicitly
* You can use `grid-auto-columns` and `grid-auto-rows` to define the size of implicitly defined columns and rows
