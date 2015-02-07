# ForwardJS Conference - Feb. 4, 2015

## Keynote - The State of the Community - Karolina Szczur

* We're all part of a community, made up of smaller communities
* Open-source built on our nature to be social
* Building up the community helps you build up yourself
  * Focus on creating safer and more welcoming spaces
* Practice empathy and gratitude
  * People work hard to make our lives easier, for free
* Realize you are more capable than you think
  * You can make an impact
* You can be a mentor
* Be compassionate when you communicate
* Always keep in mind: What are you doing for others?

## GPU and Web UI Performance - Diego Ferreiro Val, Salesforce

* Browser rendering lifecycle
  * Run JS (your code)
  * Recalculate styles
  * Layout (Reflow)
  * Paint
  * Composite layers (GPU)
* Increase performance by minimizing or removing rendering steps
  * e.g. If you only move, resize, and change opacity, you can do that w/ the
    GPU w/o any paints
    * Can force element to new GPU layer w/ `translate3d` transform
    * Use cheap operations: transform, rotate, scale, skew, opacity
* Can use Chrome timeline to measure the time in each step and FPS
  * `chrome://tracing` shows you the GPU operations
  * Can show you all the GPU layers in a 3D graphic
* Example: See ScrollerJS for performant scrolling lib
  * Touches DOM as little as possible
  * Reuses DOM elements (just replace contents)
    * Moves front item element to end of items when it goes off screen
    * Only have to paint item that was moved and had content changed
  * Uses GPU layers to minimize paints
* ADVICE: Remember to keep profiling
  * Ideal: Automate your CI to detect performance regressions
    * Selenium can measure performance today
  * Chrome performance measurements can usually be exported to JSON for
    processing in other tools

## How to Create Good Documentation - Martin Gontovnikas, Auth0

* [**Slides**](http://mgonto.github.io/how-to-create-good-documentation-talk/)
* When we write docs, we write how *we* think it is understandable
  * Have to put yourself in others' positions
* Developers are a tough audience to write for
  * Don't have a lot of time, so read through things very fast
  * Might find your docs when looking through lots of search results
* Best practices for docs:
  * Tailor docs for specific use cases (and be copy/paste friendly)
  * If possible, have tailored sample projects/examples for use cases
    * Or even a seed/starter project
  * Documentation should be easy to access (e.g. markdown on GitHub)
    * Can be converted to styled output for project companion sites (e.g.
      with markdocs)
  * Make it easy for users to help you fix and improve your docs
  * For GitHub projects, make sure READMEs are complete and thorough
    * Have Features, Installation, How to Use, API, Contributing, License
    * Key features are important to catch user attention
    * Have a TL;DR for How to Use
  * Things should be easy to find: Have a Table of Contents
    * Can have custom flows for different use cases
* Example: `auth0/docs` on GitHub

## Web Component Markup - Estelle Weyl

* [**Slides**](http://estelle.github.io/components)
* Starts with semantic, custom elements
* Benefits of Web Components
  * Style encapsulation: Styles only affect component's elements, not elements
    outside it
    * To target elements in the shadow DOM from page's CSS, have to use
      `::shadow` selector
    * To target elements in outer DOM from component, have to use the `::host`
      selector
  * Will be native instead of a separate framework
* Four building blocks
  * `<template>`
    * Contains a document fragment that produces the shadow DOM for your
      component
      * Can have scripts and `<style>` tag w/ scoped CSS
    * Behavior can be customized through attributes on custom element
    * HTML in `<template>` doesn't run, load anything, or render by default
      * Can be cloned and inserted into DOM
    * `<content></content>` specifies where elements your custom tags wraps
      will be inserted in the template
      * Can have multiple `<content>` tags if you use the `select` attribute to
        specify the selector for the wrapped element you want to pull into your
        template
  * Shadow DOM: DOM of your component, separate from main doc's DOM
    * Create instance with template's `createShadowRoot()` method, then add
      shadow root to outer DOM
  * HTML Imports:
    * Use `<link rel="import" href="...">` to load component's HTML import
      * Provides `load` and `error` events for JS
      * Repeated calls for same document won't import it multiple times
      * Can put your `<template>` in the imported file
  * Custom Elements:
    * Can create totally new element or extend existing element
      * Extending element will have extended element in prototype chain
    * Name has to contain a dash (e.g. `<blog-post>`)
    * You register a prototype and can provide callbacks
      * `createdCallback`, `attachedCallback`, etc.
      * Use `document.registerElement('tag-name', prototypeObj)`
        * Can extend existing element type with `prototype: HTMLElementType`
    * Can target custom element's shadow root in page's CSS with
      custom-tag::shadow`
* Polyfill libraries:
  * Polymer
  * x-tag
  * Bosonic


## No More Tools - Karolina Szczur

* Have we reached a tipping point w/ the amount of tools out there?
* Tools aren't what make great developers
* The best tools are *simple*
  * But what is simplicity?
* Complexity: How much time/effort it takes to understand something.
* Tesler's Law: Every application has an inherent amount of *irreducible
  complexity*
* Front-end automation
  * Oftentimes, have to run file watches to keep necessary tools running while
    you develop
  * But automation is necessary so we can focus on problem solving
  * Good tools for necessary front-end tasks
    * Autoprefixer
    * CSSLint
    * Media optimization: imageoptim-cli, svgomg
    * Minification: clean-css, cssmin
    * Code bloat: uncss, helium css
    * Perf profiling: perfmap, stress-css, YSlow, Google Page Speed
    * Debugging: Pesticide.io
* npm is good for defining scripts using package.json
  * npm, inc. is working to make it better for front-end libs
  * Adds bin files from your `node_modules` to your PATH
* Beware of tools that manage other tools

## Betting on FRP - @halacsy, Prezi

* Helps manage complex interaction
* Helps prevent problems caused by inconsistent state
* See: Deprecating the Observer Pattern w/ Scala.React
  * Paper by Martin Odersky
* Syncing between distributed/async data is hard
* Quote: "We are writing future legacy code [today]".
* Use *immutable data structures* with FRP
  * Are better for using in concurrent/threaded code
* Elm: Functional, typesafe language that compiles to JS
  * Prezi uses this now

## Building Products and Experiences that Developers Love -  Rohini Pandhi, PubNub

* [**Companion Article**](http://www.pubnub.com/blog/building-products-experiences-developers-love/)
* Sell the dream: What does your product enable them to do?
  * They care about what your product gets them, not what it is
  * e.g. Stripe, AirBrake, Heroku
* Anticipate Questions: Users may be suspicious of the value you promise and
  will have questions
  * e.g. Are the docs up to date? Will it work with my stack?
* Design w/ Purpose: Everything from UI to API should be designed intentionally
  * APIs need to be user-friendly for devs
  * Design the UI to guide users in how to use your product
    * Check out "The Hook" and Fogg Behavior Model
* Remove barriers to get users coding
  * e.g. If product is SaaS, add a free tier for trying it out
* Get feedback from real users


## A Million Ways to Fold in JS - Brian Lonsdorf, Loop/Recur

* [**Slides**](http://www.slideshare.net/drboolean/millionways)
* There are better alternatives to loops
* No loops in functional programming; use recursion instead
  * ES6 will be *tail recursive* (so can do tail-call optimization so you never
    overflow the call stack)
* Higher order functions like `reduce()` capture common recursive patterns
  * `reduce` is a general function that can be used to implement other functions
    like `map` and `reverse`
    * `reduce` a.k.a. `fold`
    * `reduce` can capture any loop
  * There's also `unfold`, which is like a `while` loop
* Transducers: A reducing function that takes other reducing functions as args
* [**Code**](https://github.com/drboolean/recursiontalk)


## On Community - Bryan Hughes, Rdio

* Based on his experience getting into Node and NodeBots communities
* Getting involved in a community is awesome, but there are some risks,
  especially for marginalized groups
* We tend to put experts on a pedestal
  * But they started somewhere too
* Keep in mind that most people are usually bad the first time they try
  something new
  * And the first version of a new software is usually bad as well
  * But that's okay
  * It's okay not to get something right the first time
* Getting involved is an ongoing process
* You don't need to be an expert on a topic to speak about it at a conference
  * More important to be passionate
  * You can find more experienced people to help you learn, but you have to take
    the risk to try it
  * We're all prone to *impostor syndrome*
* Problems in our industry
  * Misogyny, racial/socioeconomic preference, homophobia
  * If you listen, you'll hear how people are being discriminated against
* People stay or leave a project, community, or job based on how comfortable
  they feel being themselves
* Silicon Valley is the new Wall Street, and has a severe lack of diversity
  * Lack of diversity hurts productivity and other people
* Resources
  * modelviewculture.com
  * geekfeminism.wikia.com


## Homoiconicity in Javascript Through Delayed Invocation - Eric Hosick

* [**Project**](https://github.com/mechanismsjs)
* In a Homoiconic language, code can be treated as data
  * Lisp is an example of such a language
* Most of our procedural code today runs immediately
* But delayed invocation can bring us closer to homoiconicity
  * What if a call to a function didn't return a result but an object that we
    can invoke methods on to incrementally reach the result
