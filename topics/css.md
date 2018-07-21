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
  * You can pass it multiple column widths and it'll repeat each N times
    * e.g. `grid-template-columns: repeat(4, 1fr 2fr);` would give you 8 columns, alternating between `1fr` and `2fr` sizing

### Explicit Tracks

* Tracks that you've defined explicitly
  * e.g. `grid-template-columns: 200px 400px;` gives you two explicit column tracks

### Implicit Tracks

* Tracks that the CSS Grid algorithm defines for you
  * e.g. If you define `grid-template-column` but not `grid-template-rows`, all the rows will be implicit
  * e.g. If you have `grid-template-columns: 200px 400px;` and `grid-template-rows: 50px 100px;` but six grid items, the last row will be sized implicitly
* You can use `grid-auto-columns` and `grid-auto-rows` to define the size of implicitly defined columns and rows
* `grid-auto-flow` tells Grid if extra items should implicitly end up in new rows or new columns
  * The default is `row`
  * This is handy if you want to size the first column to take up all free space and the rest to take just what's needed for their content
    * `grid-template-columns: 1fr; grid-auto-flow: column;`

### Sizing Tracks

* Percents aren't the best unit, because each item will be that percent plus its grid gap
  * So you'll overflow
* The **Fractional Unit (fr)** is better for sizing tracks
  * It represents the amount of space left after all the elements are laid out
  * It is proportional
    * All columns sized at 1fr will get an equal amount of the space to distribute
  * When used with a `row` template, it won't have an affect by default since item height is based on its content
    * But if you give the grid container a height greater than all its items, they'll grow to take up the container's height
* If you use `auto` on a row or column, its items will grow to the height or width of the content of the largest item

#### Dynamically Sizing Tracks

* `auto-fill` lets grid increase or decrease the number of columns or rows based on the space available
  * It is passed as an argument to `repeat()`
  * e.g. `grid-template-columns: repeat(auto-fill, 150px)` will create as many 150px wide columns as will fill the width of the grid container
  * It causes the grid to respond to changes in viewport size
* `auto-fit` is like `auto-fill`, but it will collapse any implicitly created columns
* `minmax()` lets you dynamically size the width of a column
  * It takes a minimum size arg and a maximum size arg
    * A minimum pixel width and a maximum fractional width work well
  * It is passed as an argument to `repeat()`
  * e.g. `grid-template-columns: repeat(auto-fit, minmax(150px, 1fr))`
    * This will cause each column to be the same size and grow to fill up all available space but not shrink below 150px
    * You have to use `auto-fit`, as `auto-fill` won't grow the columns to take up all available space


### Sizing Grid Items

* `grid-column` lets you control the column sizing of a grid item
  * It can take a start column number and an end column number, separated by a `/`
    * e.g. `grid-column: 2 / 5`
    * NOTE: The end column number means up to but not including that column
    * You can use `-1` to refer to the last column, `-2` for the second-to-last column, etc.
  * You can use the `span` keyword with the number of columns you want it to take up
    * e.g. `grid-column: span 2`
  * If this causes the item to take up more columns than are left on its row, it'll break to the next row and leave empty space
    * Unless you use `grid-auto-flow: dense`, which causes items after this one to fill in the empty space before it
  * If you give the item more columns than the grid already has, it'll add extra implicit columns to the grid
  * It's a shorthand for `grid-column-start` and `grid-column-end`
    * If you give them each a specific column number, the item will start at the start number and end at the end number
      * e.g. `grid-column-start: 2; grid-column-end: 5;`
* `grid-row` lets you control the row sizing of a grid item
  * It works like `grid-column` but for rows
* You can make grid items overlay each other if you place them on the same row and column

### Grid Areas

* An area is a named section of the grid
* `grid-template-areas` defines the areas
  * It takes a string for each row
  * e.g. For a 3 x 3 grid, you could name three column areas:
    * `grid-template-areas: "sidebar-1 content sidebar-2" "sidebar-1 content sidebar-2" "footer footer footer"`
  * You can use it to define your grid instead of `grid-template-columns` and `grid-template-rows`
* `grid-area` assigns a grid item to an area by its name
  * e.g. `grid-area: footer`
* You can refer to area names when defining where a track starts and ends
  * You can refer to the start of an area with `areaname-start` and the end with `areaname-end`

### Naming Lines

* You can name the lines before and after a column or row so you can refer to them by name later
* Definition syntax: `[line-name] trackSize`
  * e.g. `grid-template-columns: [columns-start] 100px [columns-middle] 100px [columns-end];`
* Reference example:
  * `grid-column: columns-start / columns-end` to have an item take up all columns

### Aligning Things

* Grid provides several `justify-*` and `align-*` properties
  * `justify` affects the row axis
  * `align` affects the column axis
* Use `justify-items` and `align-items` on the grid container to define alignment of grid items within their tracks
  * Default value for both is `stretch`
  * Can also take `start`, `end`, or `center` values
* Use `justify-content` and `align-content` when your grid container has more space than the grid items take up
  * You can use them to vertically or horizontally center the enter group of items
  * You can also spread tracks apart with `space-between` or `space-around`
* Use `justify-self` or `align-self` to override the alignment of a particular grid item

### Ordering Grid Items

* Use the `order` property
  * It defaults to `0`, with smaller numbers coming first

### Compared to Flexbox

* Flexbox makes it easier to place a variable number of items in a row or column with one of them growing bigger than the others
  * e.g. `flex-grow: 1` on the item to grow bigger vs. `grid-template-columns: auto auto 1fr auto auto`
* Grid needs one fewer properties to vertically and horizontally center something
  * `display: grid; justify-items: center; align-content: center;`
* Only Flexbox can easily have items in wrapped rows align differently than items in previous row
  * Grid columns are more rigid
* Grid is better at consistently wrapping an unknown number of items across multiple rows
  * With `repeat(auto-fex, minmax(...));`

## CSS Variables

* `--varname: value;` declares a variable
  * Can be set on any selector
  * To apply to whole document, set on the `:root` selector
  * Can also be set via inline styles
* `property: var(--varname);` sets property to the value of the variable
  * `var(--varname, defaultValue)` will fall back to a default value if the variable isn't set
