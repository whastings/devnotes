# React Notes

## Performance

* Performance Timing Entries:
  * See component lifecycle hooks and render times in dev tools timeline
    * Add `react_perf` query param to URL
* Optimizing renders
  * By default, component will re-render whenever its parent renders
  * Can skip re-render if you implement `shouldComponentUpdate` and return false
  * Can extend `PureComponent` to skip re-render when new props shallow equal old props
    * Or use pure HOC from recompose library
    * Or use `React.memo` for functional components
  * Can also skip render if you return exact same React element as last render
* Code-splitting and lazy loading
  * Use `React.lazy` and `React.Suspense` for this
* Tools
  * why-did-you-update module
    * Logs to console when component does an unneeded re-render
  * React Dev Tools
    * Has a checkbox for "highlight updates" to show when components are rendering
    * Uses diff colors to indicate render frequency

## Elements

* Are plain object descriptions of components or HTML elements you want to add to the DOM
* Creating one doesn't instantiate a component
* Contains node's type, props, and children elements
* Tree of elements is what React diffs
* Created via `React.createElement`, which JSX produces
* Components return React elements
* See:
  * https://tylermcginnis.com/react-elements-vs-react-components/

## State

* `setState` sometimes updates asynchronously, so you can't rely on `this.state` to immediately reflect changes
  * If you need the changes reflected, use functional `setState`
    * Pass `setState` a function that receives current state and props and returns updates

## Context

* Use `static contextType` on a class for easily consuming a context
  * Then you don't have to use render prop version

## Hooks

* They enable you to use React features like state, lifecycle, and context from within a functional
  component
* You can create *custom hooks* that combine React's built-in hooks
  * Which can be shared across an app or with the community
* Names should start with "use" (e.g. `useWindowWidth`)
* They can take arguments and return values

### Motivation

* They allow you to abstract and reuse stateful logic
  * Which isn't something components are good for
  * e.g. Animations, form handling, external data connections
  * Components are better for reusing visual logic
* They help us avoid complex patterns
  * e.g. Higher-order components, render props
* They don't add additional layers and nesting to your component tree
  * Like HOCs and render props do
* They keep related concerns together, rather than split across life cycle hooks
* Will ideally make learning React simpler and easier to use
  * You'll just use functions and hooks rather than classes, functions, render props, HOCs, etc.
* "make components truly declarative even if they contain state and side effects." - Dan Abramov

### API

* `useState`: Use to create an entry in state for that component
  * Can be called many times per component, *as long as it's called in the same order*
  * Will trigger a re-render of the component when the state is updated
* `useEffect`
  * Good for:
    * Subscribing and unsubscribing to/from external sources or events
    * Perform a side-effect after re-renders

### Restrictions

* Must be called at the top-level of the component function
  * i.e. Not in a conditional or loop
    * So they're always called in the same order each time the component renders
