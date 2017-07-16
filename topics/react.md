# React Notes

## Performance

* Performance Timing Entries:
  * See component lifecycle hooks and render times in dev tools timeline
    * Add react_perf query param to URL
* Optimizing renders
  * By default, component will re-render whenever its parent renders
  * Can skip re-render if you implement `shouldComponentUpdate` and return false
  * Can extend `PureComponent` to skip re-render when new props shallow equal old props
    * Or use pure HOC from recompose library
* Tools
  * why-did-you-update module
    * Logs to console when component does an unneeded re-render
  * React Dev Tools
    * Has a checkbox for "highlight updates" to show when components are rendering
    * Uses diff colors to indicate render frequency