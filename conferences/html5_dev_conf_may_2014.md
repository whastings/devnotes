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
* Slides: https://speakerdeck.com/getify/syncing-async


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


## End to End JavaScript - Dan Lynch

* Working in all JS limits the context switching you need to do
* There is an NPM require style for AMD JS modules
* Architecture pattern: Divide code into core, sandbox, extensions, modules
  * Sandbox links modules together so they don't have to require each other
    * Also, proxies between core and modules
* **Aura**: Architecture library by Addy Osmani
* Can use **jsdom** Node module to provide document, window, and navigator vars
  * Allows you to run in Node frontend code that refers to these variables
* Can define a model's schema in one place to use on both backend and frontend
  **Inflection.js** can help with this
* Can be better to use data attributes for JS selection of elements
  * Keeps style and JS better separated than using classes
* Function's have a `length` property to indicate their arity (how many
  arguments they take).


## WebSocket Perspectives Past, Present, & Future - Frank Greco, Kaazing

* HTTP isn't optimal for event-based systems or real-time ones
  * Can be emulated (e.g. comet), but it's resource intensive and wasteful
* Websockets came around in late 2000s
  * Enables full-duplex communication over one channel
  * Made of two standards from W3C: Protocol and API
    * Protocol indicated by `ws://` (`wss://` for SSL-secured)
  * Can handle text and binary data
* Websockets now supported by all modern web browsers
* Can run application protocols over websockets (e.g. XMPP, VNC)
* Can connect web clients to other protocols via websockets
* Websockets and HTTP are the web's two major protocols
* On mobile, the persistent connection can be better for the battery,
  as it stays at the same energy level for longer
* Good for linking up web apps with different kinds of hardware
  * e.g. Arduino, phones, exercise monitor, heart rate monitor


## The Mobile Viewports - Peter-Paul Koch (@ppk)

* Slides: quirksmode.org/presentations/Spring2014/viewports_sf2.pdf
* There are more mobile browsers than desktop browsers
  * Especially, on Android
* Mobile has touch events, which are not exactly the same as mouse events
* CSS Pixel: What we use when we give a css property a px value
  * Is an abstraction on top of the real device pixels
  * Their size increases and decreases as the user zooms in and out
  * It used to be one CSS pixel per device pixel
    * Now with retina displays, you might have one CSS pixel per *four*
      device pixels
  * Everything in JS and CSS uses CSS pixels, except for `screen.width`
    and `screen.height`
* The `<html>` element's width is calculated relative to the viewport's width
  * On desktop, it equals the browser window's width
  * On mobile, the size of the viewport *changes* as zoom changes
* Layout viewport: Default mobile viewport with size between 768 and
  1024px
  * May be much wider than device's actual screen, which is why you have to
    scroll horizontally on non-mobile optimized sites
  * Responsive design involves overriding this
  * Can read its dimensions in JS in most browsers with:
    * `document.documentElement.clientWidth`
    * `document.documentElement.clientHeight`
* Visual viewport: The actual viewport of the device (its screen)
  * Can read its dimensions in JS in most browsers with:
    * `window.innerWidth`
    * `window.innerHeight`
* Ideal viewport: The ideal size of the layout viewport for a *specific device*
  * So is different for every kind of device
  * May or may not be same dimensions as physical screen size
    * e.g. Is 320px wide on iPhones, both retina and older
      * Even though retina is 640 device pixels wide
  * On Android, the most common widths are: 320, 360, 400
  * Sadly, no cross browser way to read its dimensions with JS
    * Some report ideal viewport size, others report physical screen size
* `<meta name="viewport" content="width=device-width">`
  * This sets the size of the layout viewport to the size of the ideal viewport
  * Gives us the power to do responsive design
  * Safari Bug: It uses the ideal portrait width even
    when device is in landscape
* `<meta name="viewport" content="initial-scale=1">`
  * This does the same thing
  * Sets the initial zoom to 100% of the ideal viewport
  * This fixes the Safari bug, but then IE10 has opposite problem
* Ideal solution: Combine *both* meta tags for best results
  * `<meta name="viewport" content="width=device-width, initial-scale=1">`
* Media Queries
  * Always use `min-width` and `max-width`, not `min-device-width`
    and `max-device-width`
  * Never just use `width`, as exact widths just don't work
* Watch out if using physical units (mm, cm) in CSS
  * Browsers define 1 inch as 96 CSS pixels
  * So there's no guarantee to correspondence with the real world
* Paul is writing *The Mobile Web Handbook*


## Stop Making Excuses and Start Testing Your JavaScript! - Ryan Anklam, Netflix

* Popular Node frameworks: Mocha, Jasmine, Jest, Tape
* Popular browser frameworks: Jasmine, Mocha, Jest, QUnit
* Testing dialects
  * expect (e.g. `expect(1 + 1).to.equal(2)`)
  * should (e.g. `(1 + 1).should.equal(2)`)
  * assert (e.g. `assert.equal((1 + 1), 2)`)
* Can use **Chai.js** to get all three dialects
* To test in browser, need a spec runner HTML file
* Jasmine and **Sinon.js** have good spies/stubs/mocks for functions and methods
  * Spies can tell if function was called and with what arguments
  * Can have mock return specific data
* Testing promises can be difficult
  * Can use the **chai-as-promised** plugin
    * e.g. `expect(promiseFunc()).to.eventually.equal(value);`
* Testing the DOM can be difficult
  * But you can spy on DOM element functions to see how your code
    manipulates it
  * Can also create HTML fixture files to test against
    * **jasmine-jquery** plugin helps with this (`loadFixtures` function)
* Testing async code can be difficult:
  * Mocha will pass a `done` callback to your `it` function that you call
    when your async test has completed
* Testable JavaScript:
  * Doesn't use hard-coded DOM selectors
  * Gives names to function expressions for better stack traces
  * Can be instantiated (e.g. Constructor function)
    * So you can recreate it between tests to reset state
  * Breaks up procedures into reusable functions/methods
  * Gives names to event handling functions
  * Isn't locked up in a callback (e.g. `$(document).ready`)
  * Doesn't use the Singleton pattern
* Automating Testing
  * Testing is easier when running them is easy
  * Can specify scripts to run in package.json
  * Integrate it with a build tool like grunt
  * **testem** is a runner to help you run your tests
    * Can open things you need (e.g. Phantom JS) and run your framework
    * Can open tests in multiple browsers
    * **karma** is an alternative used by Angular
