# Redux Notes

* Actions
  * Action objects should follow a consistent shape
    * See [Flux Standard Actions][flux-standard-actions]
      * Have type plus payload, error, and/or meta
  * Actions should only describe what should change, not how it should change
    * If your actions become like setters for the state, you're doing it wrong
  * Types
    * Types are global within the Redux store
      * So it can help to namespace them (e.g. `posts/SAVE_DRAFT`)
* Action Creators
* Reducers: Transforming state via pure functions
  * Reducer composition
    * Reducer can be composed of functions using any scheme for composing pure functions
    * Break up reducers so each one can manage just one part of the state tree
  * Never modify state, only return new state
  * Should return default state if previous state is undefined
  * Higher-order Reducers
    * `combineReducers` is the most famous example
    * Great for code reuse and reducing repetition
* Selectors
  * Functions for accessing data from the store's state
  * They abstract shape of the store's state so consuming code isn't coupled to state's shape
  * reselect: Library for creating memoized selectors
  * Can be co-located with Reducers as named exports ([link][colocate-selectors])
* Middleware: Customizing action dispatch
  * Learn by building example is in Egghead's Idiomatic Redux Course, lessons 16 - 18
* Normalized State
* Connected/Container Components
  * Doesn't always have to be just the one component at the top of the tree
    * Can also be components lower down in the tree (e.g. the component for each item in a list)
    * Otherwise, you end up having to pass too much data through the tree to components that need it
    * Can improve performance since react-redux connected components skip unneeded renders
* Time-travel Debugging
* Immutability

[colocate-selectors]: https://github.com/tayiorbeii/egghead.io_idiomatic_redux_course_notes/blob/master/10-Colocating_Selectors_with_Reducers.md
[flux-standard-actions]: https://github.com/acdlite/flux-standard-action
