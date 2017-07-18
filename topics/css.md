# CSS Notes

## Box Model

* Collapsing Margins
  * Top and bottom margins between two sibling block elements will collapse
    * Unless one or both create a new block formatting context (e.g. from being floated)
  * Top margin of element and top margin of first child will collapse
    * Unless you put something between them like a border
    * Bottom will too, but only if parent is `height: auto`
  * Doesn't happen with flex elements
  * See: https://bitsofco.de/collapsible-margins/