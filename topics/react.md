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
