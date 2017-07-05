# Redux Notes

## State as Single POJO

* Is your single source of truth
* Can be a hierarchy of nested objects and arrays
* Benefits
  * Easy to serialize state
    * e.g. Can send whole state along with bug report when error occurs
  * Easy to restore state later
    * e.g. Sending state computed on server to client or pulling saved state from local storage
  * No race conditions since each actions runs through single root reducer

## Read-only state

* Code can't arbitrarily mutate state wherever and whenever it wants

##  Actions

* Action objects should follow a consistent shape
  * See [Flux Standard Actions][flux-standard-actions]
    * Have type plus payload, error, and/or meta
* Actions should only describe what should change, not how it should change
  * If your actions become like setters for the state, you're doing it wrong
* Types
  * Types are global within the Redux store
    * So it can help to namespace them (e.g. `posts/SAVE_DRAFT`)
  * Some action types may correspond one-to-one with particular reducers, but it's also fine if
    multiple reducers handle the same action type
* Benefits
  * Can replay them with dev tools when debugging or developing
  * Can keep a record of them to provide to bug tracking logs when an error occurs

## Action Creators

* Can simply create action objects from input, or can do more complex (e.g. async) things by
  returning things that middleware can process

## Reducers

* Transform state via pure functions
* Never modify state, only return new state
* Should return default state if previous state is undefined

### Reducer Composition

* Reducer can be composed of functions using any scheme for composing pure functions
* Break up reducers so each one can manage just one part of the state tree

### Higher-order Reducers

* `combineReducers` is the most famous example
* Great for code reuse and reducing repetition

## Selectors

* Functions for accessing data from the store's state
* They abstract shape of the store's state so consuming code isn't coupled to state's shape
* reselect: Library for creating memoized selectors
* Can be co-located with Reducers as named exports ([link][colocate-selectors])

## Middleware

* Use for customizing action dispatch
* Learn by building example is in Egghead's Idiomatic Redux Course, lessons 16 - 18

## Normalized State

* Store data for domain models in state like you would in a database
  * Each entity lives in one place where it can be looked up by ID
  * If entity is referred to anywhere else, it's referenced by ID

##  Connected/Container Components

* Doesn't always have to be just the one component at the top of the tree
  * Can also be components lower down in the tree (e.g. the component for each item in a list)
  * Otherwise, you end up having to pass too much data through the tree to components that need it
  * Can improve performance since react-redux connected components skip unneeded renders

## Time-travel Debugging

* Useful dev workflow to replay actions as working on part of app they affect

## Immutability

* With it, you can cheaply figure out if you need to re-render something
  * Just check if `newData !== previousData`

## Testing

* Decoupling what happened (actions/action creators) from what should change (reducers) is awesome
  for testability

[colocate-selectors]: https://github.com/tayiorbeii/egghead.io_idiomatic_redux_course_notes/blob/master/10-Colocating_Selectors_with_Reducers.md
[flux-standard-actions]: https://github.com/acdlite/flux-standard-action
