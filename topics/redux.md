# Redux Notes

## Design Goals

* Know when, where, why, and how state changed
* Support DX and productivity
  * Especially time travel and hot reloading
* Introduce FP principles
  * Function composition, purity, immutability
* Help you write testable code
* Explicit data flow and updates
* Minimal but extensible API
  * "We should favor patterns and conventions over rigid, privileged APIs." -Dan
  * Excludes extra features like Ajax so it can stay simple and move fast in dev
    * Instead added middleware extension point so devs can create or choose desired implementation
    * As opposed to prescribing particular solution by including it in core
* See: http://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/

## State as Single POJO

* Is your single source of truth
* Can be a hierarchy of nested objects and arrays
* Best if it's plain JS objects and arrays or things that can be serialized to them
* Types of data in state:
  * Domain data: i.e. models, e.g. todos
  * App state: e.g. is loading data
  * UI state: e.g. modal is open
    * Note: It's fine to keep UI state local to a component if it isn't used outside it and wouldn't
      be that valuable for time traveling
      * e.g. Input values in a simple form
* Don't try to make state shape match UI structure
  * Instead, design shape around domain data and app state
* Benefits
  * Easy to serialize state
    * e.g. Can send whole state along with bug report when error occurs
  * Easy to restore state later
    * e.g. Sending state computed on server to client or pulling saved state from local storage
  * No race conditions since each actions runs through single root reducer

## Read-only state

* Code can't arbitrarily mutate state wherever and whenever it wants
  * Makes tracking down where state changed a lot easier
  * Can't make UI code concerned about how state should be changed

## Unidirectional Data Flow

* Inherited from Flux
* All data updates follow same lifecycle
* Makes state change deterministic and understandable

## Actions

* Are objects that describe what should change about app's state
* Action objects should follow a consistent shape
  * See [Flux Standard Actions][flux-standard-actions]
    * Have type plus payload, error, and/or meta
* Actions should only describe what should change, not how it should change
  * If your actions become like setters for the state, you're doing it wrong
* Types
  * String describing an action
  * Usually separated out into a separate JS constant for easy reference in the reducers
  * Types are global within the Redux store
    * So it can help to namespace them (e.g. `posts/SAVE_DRAFT`)
  * Some action types may correspond one-to-one with particular reducers, but it's also fine if
    multiple reducers handle the same action type
  * Naming:
    * Can be named as commands (e.g. `RECIEVE_TODOS`) or events (e.g. `TODOS_RECEIVED`)
* Benefits
  * Can replay them with dev tools when debugging or developing
  * Can keep a record of them to provide to bug tracking logs when an error occurs

## Action Creators

* Functions that create action objects
* Can simply create action objects from input, or can do more complex (e.g. async) things by
  returning things that middleware can process
  * e.g. Making Ajax request or checking current state
* Action creators can dispatch multiple actions in a row, but this can cause lots of re-rendering
  * With render optimization, this is less of a concern than keeping actions as "what" not "how"
* Easiest to use in components if you bind them to dispatch with `bindActionCreators`
  * Then component can be agnostic of Redux
* Some middleware give action creators full control over reading from state and dispatching actions
  * e.g. redux-thunk and redux-saga
  * Some say this is too powerful and creates a "foot gun"
    * Can end up dispatching lots of setter actions
      * Stop describing what happened and start controlling how state should change
      * Results in lots of re-renders
      * State can be in invalid state in between actions (don't treat multiple dispatches as a
        transaction)
    * Could instead use a middleware that creates actions for you
      * Like one that creates pending, resolved, and rejected actions for a promise (e.g.
        redux-pack)
  * Best if you just read from state when you need to do a conditional dispatch
    * e.g. See if some data is already loaded
  * Avoid pulling state and putting it in action object, as it obscures source of data
    * Instead, prefer to build action object just from args passed in to creator or data returned
      from async call
* Benefits
  * Keeps creation of actions DRY and reusable
  * Can keep loading data from and sending data to APIs DRY and reusable
  * Keep data loading/update and other business logic out of UI layer

## Reducers

* Transform state via pure functions
  * Should be pure: Don't want side effects running when actions are replayed
* Never modify state, only return new state
* Should return default state if previous state is undefined
  * Root reducer invoked with undefined state on app start
  * Or if `createStore` is provided `preloadedState`, it will override reducer default state values
* Only root reducer must have `(state, action) -> newState` signature
  * Can pass more or different args to sub-reducers if useful
* Slice: One part of a piece of state
  * Common to have a sub-reducer for each slice of the root state object

### Reducer Composition

* Reducer can be composed of functions using any scheme for composing pure functions
  * e.g. Can create reusable function for implementing common operations
* Break up reducers so each one can manage just one part of the state tree
* Each nested reducer can define its own initial state

### Higher-order Reducers

* Function that possibly takes a reducer and returns a reducer
* `combineReducers` is the most famous example
* Great for code reuse and reducing repetition
  * e.g. One that creates a reducer for managing a list of entities

## Selectors

* Functions for accessing data from the store's state
* They abstract shape of the store's state so consuming code isn't coupled to state's shape
* reselect: Library for creating memoized selectors
  * Create selector by giving it one or more functions to select data and a function to do something
    with selected data
    * The "do something" function only runs when select data changes (new references/values)
  * Can use one memoized selector as input to another
* Can be co-located with Reducers as named exports ([link][colocate-selectors])

## Middleware

* Functions that can intercept dispatched actions
* Use for customizing action dispatch
  * Particularly allowing things other than action objects to be dispatched (e.g. functions,
    promises)
* Middleware run in a chain, with each calling a `next` function to invoke the next middleware,
  passing the current value of the action
  * Any middleware in the chain can call `dispatch` and start the process over
  * Can also run logic after action dispatch is complete (after calling next)
* Learn by building example is in Egghead's Idiomatic Redux Course, lessons 16 - 18

## Normalized State

* Store data for domain models in state like you would in a database
  * Each entity lives in one place where it can be looked up by ID
  * If entity is referred to anywhere else, it's referenced by ID
* If entity order needs to be stored, store an array of entity IDs, leaving entity objects in a
  by-ID map
* redux-orm: A library that provides a nice OO facade over working with normalized data
* Benefits
  * Helps keep UI state separate from model data
  * Prevents data duplication
  * Makes update logic simpler since less nesting
  * Less re-renders since less objects are recreated on update
  * Works well with nested container components
    * Parent can just pass ID to container and it can look up object

## Connected/Container Components

* Provide data and behavior to presentational components
* Ideally, have none or little markup just for structure and no styles
* Doesn't always have to be just the one component at the top of the tree
  * Can also be components lower down in the tree (e.g. the component for each item in a list)
  * Otherwise, you end up having to pass too much data through the tree to components that need it
    * Connected components can grab the data they need out of the state so their parents don't have
      to provide it
    * Watch out for components that take props just to pass them down
  * Can improve performance since react-redux connected components skip unneeded renders
* React-Redux connected components prevent unneeded re-renders by doing shallow equality checks on
  props from `mapStateToProps`
  * Doesn't even invoke `mapStateToProps` if state object is the same
  * Be sure to return same object references as props whenever possible
  * If you need to do filtering, sorting, or other deriving of data, use reselect to memoize the
    work
  * `mapStateToProps` should run as fast as possible
* Will be asked to re-render after each action dispatch
  * Can use middleware to batch action processing

## Time-travel Debugging

* Useful dev workflow to replay actions as working on part of app they affect

## Immutability

* With it, you can cheaply figure out if you need to re-render something
  * Just check if `newData !== previousData`
* When updating state tree, only things affected by a change need to be copied
  * Only the thing that changed or things containing the thing that changed
* Redux and react-redux rely on immutability to know when something has changed

## Testing

* Decoupling what happened (actions/action creators) from what should change (reducers) is awesome
  for testability
* Plain action creators and reducers are really easy since they're pure functions
* Async action creators can be tested using a mock store (see redux-mock-store)

[colocate-selectors]: https://github.com/tayiorbeii/egghead.io_idiomatic_redux_course_notes/blob/master/10-Colocating_Selectors_with_Reducers.md
[flux-standard-actions]: https://github.com/acdlite/flux-standard-action
