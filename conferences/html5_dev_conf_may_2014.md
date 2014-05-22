# HTML5 Dev Conference, May 2014 Notes


## Web Security in Node.js and JavaScript Apps (SPAs) - Mark Stuart, PayPal

* XSS has caused problems for PayPal, LinkedIn, etc.
  * It's a real problem
* Companies including PayPal and Walmart are migrating from Java to Node.js
* Securing Node.js
  * Know what you `require`
    * Anyone can publish to NPM
  * **Krakenjs**: Good conventions on top of Express for large scale apps
    * Express isn't super secure out of the box, as it is bare bones
    * **Lusca**: App security module for Express
      * Gives you middleware like csrf, xframe, xssProtection
      * Also supports *Content Security Policy* (lets you whitelist page
        resource sources)
        * Hacker can't inject code to load script from non-whitelisted host
  * If you can't handle an error, best to just restart your app
    * Can use **forever** module
  * Avoid templating engines or other libraries that use `eval`
    * Too insecure when outputting user input
  * Follow **Node Security Project** (nodesecurity.io)
    * Mission is to audit all Node modules and publish vulnerabilities
    * Can use **validate-package** Grunt task to check for vulnerable dependencies
      * Module: grunt-nsp-package
  * **ESLint**: Like jshint, but allows custom rules
    * Can write rules to detect security issues
* Securing cookies
  * Make cookies `HTTPOnly` so injected JS wouldn't be able to read them
  * In Express, can set it on session middleware
* Securing client-side JS
  * Pretty much comes down to content injection
  * Escape all user input when outputting, as well as backend data to be safe
  * Know how your templating library escapes output
    * e.g. In Underscore, use `<%- ... %>` to escape output
  * Make sure to upgrade your front end dependencies
    * **Retire.js**: Scans frontend dependencies for vulnerable versions
      * Can use *grunt-retire* plugin


## Syncing Async - Kyle Simpson

* Author of: *You Don't Know JS* (youdontknowjs.com)
* *Event-Loop Concurrency*: JS mechanism for handling async events
  * Involves breaking up a bigger task into separate, async steps
  * Can handle multiple bigger tasks at once by processing async steps from all
    of them
* Async patterns: Coding patterns that help us think about async control flow
  in a more synchronous manner that our brains are used to
  * Callbacks
    * Problem: *Callback Hell*
      * Its true issue involves Inversion of Control: You have to trust the
        function to run the callback at the right time, the right amount of
        times, with the correct context, handling all errors, etc., etc.
      * Very hard to solve its problems with just callbacks
  * Promises
    * Is an "IOU" for a function's result
    * Is an object on which you can register event handlers for when the operation
      completes
      * e.g. complete, success, error
      * So promises still take callbacks
    * Promises solve inversion of control issue of callbacks by
      * Making sure promise is only *resolved* once
      * Only calling success or the error callback is invoked, not both
      * Keeping result of function immutable once promise is resolved
    * ES6 will have native promises:
      * `new Promise(function(resolve, reject) { ... })`
      * Already available in some browsers
      * Are polyfills available; Kyle has written one
        * github.com/getify/native-promise-only
    * Are easy to chain together
      * Makes it easy to ensure callbacks are called in desired order
    * Can manipulate them with Array's `map()` and `reduce()`
    * Can use `Promise.race` to race two Promises and handle whichever
      completes first
      * Good for implementing a Promise timeout, as you can create a Promise
        with a `setTimeout()` call
    * **Asynquence**: Kyle's library for automatically creating and
      chaining promises
      * Is an example of cool abstractions you can write on top of promises
        to make the syntax more concise
  * Generators: New language feature in ES6
    * See video of other talk at this time on just generators
    * Helps you hide async as an implementation detail
      * Uses `yield` keyword to call async function or handle a promise
    * **Regenerator**: A transpiler by Facebook that compiles generator
      code to ES5 compatible code
  * ES7 will likely have built in async functions


## Polymer, the Gateway Drug to Designer-Developer Happiness - Rob Dodson

* Thinking in *components* leads to better design
  * Components can be rearranged to respond to different devices
  * Components can be *composed*
  * Components can be tested separately
  * Components today are hard to work with because we have to copy
    and paste markup
    * Frameworks like Angular have ways to define reusable components,
      but they're framework-dependent
  * Style Tiles are a good way to start thinking in components
* Web Components: Emerging standards for extending HTML
  * Can create custom HTML tags that encapsulate a component's markup and style
  * Shadow DOM: The isolated DOM behind a web component
    * e.g. All the controls on an HTML5 `<video>` element
  * New `<template>` tag for holding HTML templates
  * HTML Imports: Mechanism to load components into HTML
* Polymer: Library that enables use of web components in modern browsers today
  * polymer-project.org
  * IE 10+
  * You can start playing with them now to give browser makers feedback
  * Comes with core elements predefined (lists, menus, etc.)
  * Create custom element with `<polymer-element>` tag
    * Put `<template>` tag inside it with element's html and `<style>`s
      * CSS in `<style>` is scoped to the custom element's markup and
        won't affect markup outside the element
    * Can also include `<script>` tag to register element with Polymer's JS
    * Can include the content in your element's markup with `<content>` tag
      * Content is the stuff put between your element's starting
        and ending tags
  * Includes declarative event bindings for custom elements
    * e.g. `on-tap="..."`
* Slides: robdodson.me/polymer-html5devconf
