# ForwardJS, July 2014, Notes


## Algorithms for Animations

* By Courtney Hemphill, Carbon Five, @chemphill
* Motion perception
  * Animations are cognitive aids
  * **Affordance**: An assumption you make about physical objects
    * Is something you can translate to a UI
* Animations in UIs
  * Subtle motions help users understand how to use a UI
  * Can use animations to guide people through a process
  * Can use animations to highlight the most important/relevant info
  * Design animations so they act like people expect things to do in the
    physical world
* Formulas for animation
  * interpolation: valueAtTime = (end - start) * time / duration + start
    * change = end - start
    * percent complete = time / duration
    * But things in real world aren't linear
  * Easing Function:
    * Simplest: `Math.pow(percentChange, 3)`
    * Can use trig to make them feel more natural
  * Elasticity Function:
    * Gives you a bounce back effect
  * Bounce Function
* Frameworks & Tools
  * Framer.js
  * Tween.js
  * GSAP (Greensock)
  * Animate.css
* CSS animations are great for scaling, rotation, translation, & opacity
* References:
  * Easing Functions by Robert Penner
  * Google's Material Design Handbook


## Virtual JavaScript Machines

* By Guillermo Rauch
* **JS Linux**: A JS project that can run the Linux kernel in the browser
  * Uses buildroot and busybox to produce tiny Linux image for the browser
    * buildroot lets you add frameworks like Node and PHP
    * Busybox gives minimal GNU tools
* Could be a great way for demoing Node based projects right on the web
* jsvm.io offers a tool for configuring Linux images for JS Linux
* He is starting **JSVM**, since JS Linux isn't open source


## Designing Animations & Transitions

* By Andi Galpern, @andigalpern, from CascadeSF
* "Animation is visual music"
  * Good animations have a good rhythm
* Animations composed of timing and spacing
* Better to have something fade in than appear immediately on hover
* If animation is too fast or doesn't have any easing, it doesn't feel
  comfortable
* Don't want an animation to be:
  * Too fast
  * Too linear
  * Too jarring
* Good animations:
  * Are fun
  * Delight the user
* Use animations to
  * Introduce a page or element (e.g. graph)
  * Emphasize a component
  * Guide the user from point A to point B
  * Make an important thing obvious
* Resource: google.com/design/spec/animation/meaningful-transitions.html


## Testable Angular

* By Ari Lerner, Fullstack.io
  * Author of ng-book and ng-newsletter
* Types of tests for front-end: unit tests, end-to-end
  * End-to-end automates interacting with the site
  * Unit testing doesn't have to be browser-based, but end-to-end does
* Better to write code that's testable than whether or not you do TDD
* Isolate functionality into separate methods so it's testable
* **Karma**: The Angular test runner
* To unit test Angular
  * Inject your app's module: `beforeEach(module('yourApp'));`
  * For unit testing a controller, get access to the $scope so we can set
    expectations on it
    * Need to inject the controller and scope like Angular usually does for you
      * `beforeEach(inject(function($rootScope, $controller) {...`
      * Create a new scope: `scope = $rootScope.new();`
      * Instantiate controller:
        * `MyController = $controller('MyController', {$scope: scope});`
          * Provide dependency values after the controller name, making it easy
            to inject mocks
  * There's a mock for `$http` called `$httpBackend` that comes with
    angular-mocks
* End-to-end testing with Angular
  * **Protractor**: Tool for Selenium front-end testing for Angular
    * Gives you access to a `browser` object in your tests
      * Has methods to visit pages, click links, etc.
    * Can use `by.css('selector')` to select elements for expectations


## The Low Down on Web Components

* By Erik Bryn, @ebryn
* Web Components are a group of standards for making reusable code
  * Mostly pioneered by the Google Chrome team
* **Custom Elements**: A standard way to create your own HTML tags
  * Can make them today with `document.registerElement`, giving it a JS
    prototype with certain methods defined
    * Your element's prototype should inherit from `HTMLElement.prototype`
    * You define `createdCallback`, `attachedCallback`, `dettachedCallback`, and
      `attributeChangedCallback` methods
* **HTML Templates**:
  * Uses the `<template>` tag
  * They don't display anything on screen
  * All HTML inside them doesn't execute
  * In JS, can call `template.content.cloneNode(true)` to run its contents
* **HTML Imports**: Let you include HTML documents in other HTML documents
  * Uses link tag: `<link rel="import" href="other-doc.html">`
  * Common dependencies are only loaded once
* **Shadow DOM**: Encapsulates a sub-DOM that's segregated from its parent DOM
  * Enables encapsulation of the DOM
  * Will isolate its styles
  * Can create a shadow DOM in an element with `element.createShadowRoot()`
* Web Component support will be in Angular 2.0
* Hopefully they'll be a way for code to be reused between frameworks
* Google's polyfill for modern browsers is **platform.js**


## React and Flux

* By Bill Fisher and Jing Chen, Facebook
* **Slides**: speakerdeck.com/fisherwebdev/fluxchat
* **Unidirectional Data Flow**:
  * Keeps app simpler
* **React**: A rendering library
  * Uses a virtual DOM for speed, diffing it with the real DOM to only make real
    changes
  * Create template with `React.createClass`
  * Has `setState` method which triggers a render
  * Define the template in the `render` method with JSX
* **Flux**: A MVC library
  * Stores: Like models, contain data and business logic
  * Controller-Views: Listen to change events on Stores


## The Lesser Known Features of ECMAScript 6

* By Bryan Hughes, @nebrius, Rdio
* Big things in ES6 are: Classes, modules, promises, generators
  * But he's not talking about them
* The `let` keyword will allow you to declare block-level local variables
* Template string literals
  * Wrapped in backticks
  * Insert expression with `${}`
* Computed property name: `{ [variable]: 'value' }`
  * Property name will be value of variable
* Shorthand functions: `{ foo() { console.log('hi'); } }`
* Variadic parameters: `function aFunc(param1, param2, ...arrayOfRest) {...`
* Spread parameters: `someFunc(arg1, arg2, ...arrayOfRest);`
  * Breaks array up into multiple arguments
* `for...of` for iterating over arrays
* Array destructuring: `let [x, y] = [1, 2];`
* Object destructuring: `let { key1: var1, key2: var2 } = object;`
* Google's **Traceur** will compile ES6 code to ES5 code
* **Slides**: slidesha.re/1nFBm5D


## Embracing Failure on the Front End

* By Clay Smith, @smithclay, PagerDuty
* Can listen for errors with `window.onerror`
* **Phantomas**: PhantomJS-based tool for collecting performance metrics and for
  monitoring a page
  * Are projects to use Netflix's Chaos Monkey for the browsers
* Are tools for collecting data on JS errors on your pages
  * See: blog.meldium.com/home/2013/9/30/so-youre-thinking-of-tracking-your-js-errors
