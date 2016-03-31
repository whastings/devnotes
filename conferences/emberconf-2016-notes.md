# EmberConf 2016 Notes

## Keynote - Tom and Yehuda

* Last year, introduced Ember 2.0
* Three release channels: Canary, Beta, Stable
  * Now have an RFC process for changes
  * Also have an LTS channel for stability
* Yehuda wrote "Refining the Release Process" RFC to improve problems they
  encountered in the 2.0 release
* Ember is strong because it has lots of companies sponsoring development
* Ember leadership changes
  * Core Team will continue to guide overall direction
  * Teams established for specific domains (Ember Data, Ember CLI, etc.)
  * New Learning Team dedicated to educating devs
* New community contributions
  * Ember Twiddle for demoing Ember code online
  * ember-concurrency for managing async code with generators
  * ember-redux for using redux for data
* The future of the web
  * Focused on mobile because web is great on the desktop but not on phones
  * Many companies push their native apps hard on their mobile websites
    * Google found that on Google+ the interstitial caused 69% bounce rate
    * Google will now penalize web apps with interstitials on mobile
  * Many of the strengths of native apps are coming to web
    * Service workers will enable offline support, push notifications, etc.
    * We as devs need to use these new features so vendors will prioritize them
      * Some people avoid new features because they don't want to increase their
        download size, and so vendors don't put as much work into them
  * To address load time, Ember is pursuing FastBoot, Svelte builds, Engines,
    Service Workers, App Cache (for SW fallback)
    * Svelte builds will allow you to build Ember w/o features you don't use
      * Want to support tree shaking
    * Engines will allow you to split app into bundles that can be downloaded
      separately as the user changes routes
    * Will use Service Workers for offline access and eager resource fetching
    * People hate App Cache, but it has great browser support and not everyone
      has access to Service Workers
      * It's actually pretty good at caching assets for offline access
* Web features and tools need to be easy to use or many people won't have time
  to learn them
  * So Ember is focused on making tools, like FastBoot, easy to use
* Glimmer 2
  * Has built-in component support, unlike HTML Bars
  * Is about twice as fast on the DB Mon stress test as HTML Bars
  * Also need to work on optimizing the Ember object model
  * Templates are 5 times smaller than HTML Bars
* Ember is an "SDK for the Web"

## Using Service Workers in Ember - John Kleinschmidt

* Improves on the offline support that App Cache initially provided
* But they can do much more
* Is a network proxy to control every network request
* Includes a `Cache` API for controlling cached resources
* `fetch()` is complimentary to service workers
  * No XHR in Service Workers, have to use fetch
* Run in a separate thread like Web Workers
* Only work on https or localhost
* Check out: `jakearchibald.github.io/isserviceworkerready`
* Check out: `serverworke.rs`
* Dev Tools supports debugging them and viewing cache contents
* Caveat: Hard refresh will disable your SW
* Check out: `broccoli-serviceworker`
  * Will set up your Ember app for offline use
  * Can configure which routes are "network first"
  * Can configure what to "precache"
  * Includes Google's Service Worker Toolbox
    * Has built-in network strategies (network-first, cache-first, etc.)
  * Can define service workers in `app/serviceworkers`
* Browser can give you network type (e.g. 2G) info that you can use in service
  workers to optimize requests
  * e.g. Request fewer entries on slow connections
* SW will give us native push notifications
* Can queue up and defer requests when user is offline
* Unit testing service workers is still hard, but there are efforts underway
* We need to think about how Service Workers can fit into Ember core

## Cross-Pollinating Communities: We all Win - Chris Ball

* "Good artists copy; great artists steal"
* "We are all influenced by what comes before us and what is around us"
* Communities Ember "stole" from
  * Ember took a lot from Sprout Core initially
    * Built by Apple and based on Cocoa
    * Had a desktop-style MVC architecture
    * Had prop mutation observers and computed properties
    * Sprout Core 2.0 became Amber.js which became Ember.js
  * Rails
    * It popularized convention over configuration, which Ember adopted
      * Ember devs can understand any Ember app
    * It popularized strong CLI tools, and now we have ember-cli
    * Rails also pioneered a strong router
    * DSL inspiration for things like Ember's acceptance testing
  * React
    * Taught Ember a lot about rendering
    * Ember is now moving towards one-way bindings
      * Now Ember has the philosophy of "Data Down, Actions Up" (similar to
        Flux's unidirectional data flow)
    * React's virtual DOM diffing paved the way for Glimmer
* Communities that stole from Ember
  * React Router
    * They state the inspiration they got from Ember's router
  * Angular is basing their cli on Ember's
* Ember's influence in the industry
  * Went all-in on promises
  * Went all-in on components
  * Went all-in on ES6 using Babel
  * Ember Data is lending support to the JSON API spec

## Living Style Guide Driven Development - Chris LoPresto

* Living styleguide: Design showcase built using live codebase
* Check out: Inspiration via manifesto article
* Components lend themselves to developing styleguides
  * Better to build with production components instead of mockups
* Allows for rapid prototyping and feedback
  * Can start working on components before app design/infra is ready
* Can work with components that might be hard to examine in an app
  * e.g. Loaders, empty states
  * You can make sure these components aren't neglected
* You may develop better components when you develop them in isolation from a
  specific app
* Good for figuring out edge cases in advance
* Makes for great onboarding docs for new devs
* Is a great talking point for working with designers
* Check out: ember-freestyle
  * Addon for adding a style guide to your Ember app

## Warp Speed Memory Management - Kelly Senna

* You can't expect users to have optimal connection and hardware
* Memory life cycle: allocate, use, release
  * Lower-level languages have primitives to control this manually
* Garbage collection
  * Generational: Divides objects into young and old groups
    * Objects that live past GC in the young group are moved to old
    * GC for young runs more frequently than GC for old
* Memory issues can crash tabs and deplete battery life
* Memory leaks still happen frequently
  * Happens when you keep a reference to an object around too long and the
    browser can't GC it
* Ember memory optimizations
  * Uses top-level DOM event listener to keep listeners at the minimum
  * Uses a run loop to schedule work (e.g. batching DOM changes)
    * Handles syncing bindings, running actions, transitioning routes,
      rendering, removing destroyed objects
  * Provides component lifecycle hooks to do work at appropriate times
  * Helpers for event observers that take care of unsubscribing to prevent
    memory leaks
  * Computed properties to compute and cache values

## Ember at Scale - Chad Hietala

* LinkedIn looked to Ember to enable us to create new experiences
* You can't decide on a framework just on perf benchmarks
* We try to share as much infra as possible
  * e.g. Company-wide schemas for tracking events
* We liked that Ember provides guarantees about how it will change in the future
* Problems we've solved
  * Initial render
    * Created our BPR to do SSR and bigpipe data streaming
    * Facebook coined the term "big pipe"
    * Big pipe: BPR runs Ember app on server to determine data calls for initial
      renders, makes the calls, and streams response along with the HTML of the
      initial response
    * Early flush CSS and script tags to start asset downloads quickly
    * Check out: ember-prefetch
      * Allows all child routes of parent routes to fire API requests in
        parallel
  * Serving app's with the BPR
    * We use a "sidecar architecture": Run main app process next to another
      process it works with
      * e.g. We run Java and Node in the BPR
    * Our Java BPR server spawns Node processes for requests to run the app
      server-side
    * Node runs app that makes API requests and Java server proxies requests
      to our API servers
  * Deployments
    * Publish ember-cli build assets to CDN and create a BPR instance for the
      deployment
      * The BPR instance is what's deployed
  * V8 JS Engine
    * Check out: chrome-tracing npm package
      * Launches Chrome with flags to give you info on V8 by parsing its logs

## ember-cli: The Next Generation - Stefan Penner

* ember-cli introduced two years ago at EmberConf
* Has set the bar for front end dev tools
* ember-cli has done well in terms of issues closed, downloads, addons created
* There's room for improvement in ember-cli's stability
* Check out: #dev-ember-cli on Ember Slack
* Looking to migrate away from Bower to all npm dependencies
  * Will have separate addon to support bower
* Engines will give us obvious bundle boundaries for breaking up app.js
* Will try to get tree shaking with Linker module
* Need better asset rewriting to support base href
* Why Broccoli
  * Is just an asset pipeline
  * Supports a build pipeline for each addon
  * Uses the file system as the interface between plugins
  * Need to make Broccoli and plugins use the filesystem more efficiently
    * Already have patch rebuilds that speeds things up a lot
    * We have instrumentation tools to measure perf (e.g. broccoli-viz)
  * Plugins spend a lot of time diffing input directories
    * Would be better if they tracked changes
  * Need to build better plugin helpers
* Check out: Perf Guide in ember-cli repo

## Easy-Bake Testing - Liz Baillie

* QUnit is the default testing library for Ember
* Ember Test Helpers give you acceptance testing methods (e.g. `visit()`)
  * Have async helpers and sync helpers (e.g. checking the DOM or current URL)
  * Can use `andThen()` to do something after all async helpers
* Testem comes with ember-cli
* ember-cli supports unit, integration, and acceptance tests
  * Integration tests used a lot to test components
    * Test how one layer of app interacts with another (e.g. component with
      templates)
  * Acceptance tests let tests interact with apps like a user
* Mocks, spies, and stubs are handy for unit tests
  * Sinon is great
  * Check out ember-sinon and ember-sinon-qunit
  * Stubs are spies with custom behavior
* Mocking API responses
  * Check out: Mirage
    * Can use in dev or test
    * Support for data factories
* Can use other testing frameworks with addons
  * Jasmine is great if you like TDD
  * Chai is great for assertions (lots of assertion options)
    * But hard to get working with Ember in QUnit
  * Mocha supports any assertion framework
    * But no arrow functions for test callbacks (needs Mocha context)
* Page Objects
  * Abstract accessing your UI's structure (e.g. selectors) in one place so
    tests won't be brittle due to repeated selectors that may change
  * Check out: ember-cli-page-object
  * Can encapsulate UI structure in one object (e.g. `enterNewPassword()`)
* Writing your own test helpers is great for keeping tests DRY
  * Ember supports custom test helpers

## The Ember Addon Community - Katie Gengler

* Check out: ember-feature-flags
* ember-cli has had addons for nearly two years
* Over 2,500+ addons published
  * Estimated that 1,400 are active
* Every addon has the `ember-addon` keyword
* Check out: `emberaddons.com` and `emberobserver.com`
  * Ember Observer gives addons a score based on popularity, activity, docs,
    presence of tests, etc.
* Have to choose carefully because dependencies are liabilities
  * Helps to check addon demos
  * Make sure addon is maintained
  * Check code and API for gotchas
  * Check issues and PRs for open problems
* Being a good maintainer
  * Write tests
    * Can test against various dependency versions with ember-try
    * Good to test against canary
  * Bring in as few dependencies as possible
  * Write docs
    * People need a good idea of what your addon does
  * Follow semver
  * Make sure to reach 1.0 at some point
  * Be careful when using private Ember APIs
    * This is sometimes necessary to experiment with new solutions
  * Make sure your production build doesn't add a lot of log output
  * Watch out for new Ember deprecations that could affect your addon
* Addons can be great proof-of-concepts for new Ember features
  * e.g. ember-concurrency, ember-engines, ember-computed-decorators
* Addons are used to polyfill old, sometimes deprecated behavior
  * e.g. ember-legacy-views

## Immutability Is for UI, You and I - Charles Lowell

* Old approach: reactivity via property observation (e.g. Backbone models)
  * Becomes unmaintainable as app grows
  * Ember's object class helps with this with things like deeply nested property
    observation
  * You changed the view by updating the model
    * Because the view observes the model
* Makes it easier to debug state changes over time
* Computed properties can be a burden when used with complex data structures
* With immutability, you get a "stream" of discrete state objects instead of an
  ever-changing single object
  * Similar to a movie, which is made up of discrete frames
* Steps to utilize immutability
  * Bundle all state in one object/data structure
  * Replace the whole model with every change
* Since the state object doesn't need to change, it can be a POJO
  * Doesn't need to support observability
  * Makes debugging output much nicer
  * Also makes stack traces much simpler
  * You can use es5 getters instead of computed properties
* With immutability, you can know what happened when and why
  * Each event that changes state maps to one state object
* Makes implementing undo/redo trivial

## How to Build a Compiler - James Kyle

* People tend to be scared of getting into compiler work
* Compiler transforms source code from higher level language to (usually) lower
  level target language
* Source code doesn't map directly to machine operations, so has to be compiled
* General compiler steps
  * Parsing
    * Lexical analysis
      * Breaks source code into tokens
    * Syntactic analysis
      * Organizes tokens in a data structure (Abstract Syntax Tree) made up of
        nodes (e.g. literal, call expression)
  * Transformation
    * Traverses and manipulates the AST
    * Can keep AST in the same language or transform it to another
    * Can add, remove, or change nodes
    * Traversal goes through AST depth-first
      * Breadth-first wouldn't go in order of the source code
  * Code generation
    * Turns AST back into a string (or binary)

## The Future of Ember Templating - Yehuda Katz and Godfrey Chan

* Ember has always preferred declarative templates
  * Templates behave like pure functions
* Glimmer 2 written in TypeScript
* Based on References: a representation of a value or computation result that
  may change over time
  * Help with implementing things like lazy evaluation
  * In Ember, references are behind curly-brace expressions
  * Also helps with caching (don't recompute unless inputs change)
* Glimmer uses a system called Validations to determine what data is fresh
  * Based on an interface called `EntityTag`
* For more details, check out: `bit.ly/glimmer-guides`
* Glimmer parses templates into an AST like structure
  * Done at build-time to reduce clientside work
  * Is a JSON-like structure in Glimmer 2, but was JS code in Glimmer 1
    * Now browser doesn't have to eagerly eval all templates because they aren't
      code
    * Now runtime compilation produces code from data structure
      * Also figures out what curly expressions refer to (e.g. data, helper,
        component)
* Glimmer only checks dynamic parts of templates for changes
  * And uses revision tagging to minimize how many references it needs to check
  * Also doesn't recheck a reference when the value passed is constant (e.g. a
    string literal)
    * Can use the `unbound` helper to mark an input as constant
* Can also optimize components that only consist of templates (class-less)
  * e.g. Can inline component's template result if input is constant
* Chunked Rendering
  * Split up work of rendering so browser regularly gets back control to prevent
    jank
  * Is coming soon
* Glimmer will make rehydration easy
  * Due to its `ElementStack` interface

## Ember Between Design and Development - Lisa Gringl and Francesco Novy

* They work for Cropster (market for coffee producers)
* Collaboration
  * Old way: Designer creates design and gives it to dev to implement
    * Gets harder as more and more changes are made to the design
  * Better workflow: Designer produces static HTML and CSS, then developer
    implements templates and JS
* Why should designers code (HTML and CSS)
  * Less likely to create design that's hard/impossible to implement
  * Less likely dev won't implement design as designer intended
* Why should devs understand design
  * We should care about design and question designs given to us
    * "Does this design make sense?"
    * "Is it consistent with other designs?"
* Documentation
  * Helps maintain consistency, speeds up workflow
  * Docs should be integrated into project so they're always up-to-date
    * e.g. Docs generated from docblock comments for classes, attributes,
      methods, etc.
  * Living styleguide
    * Living = autogenerated from app code
    * Should be an inventory of your UI elements
    * Check out: `broccoli-livingstyleguide`
      * Integrates with Sass
      * Parses markdown files that match up with Sass partials
  * Component guide
    * Shows your Ember components and how they can be used
    * Building one is a good way to make sure your components are reusable and
      encapsulated
