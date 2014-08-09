# Projects

## Heapsort

* Rails JSON API on backend
* Backbone on front-end
* Recursive SQL
  * `Category#ancestors`
* Cache columns
  * `Resource#up_votes_count` and `Resource#down_votes_count`
* Decorators via Draper gem
  * Add methods for formatting data for JSON output
* Marionette Components (View, ItemView, CollectionView, ItemView)
* Custom Backbone extensions (CompoundView)
* Responsive design via mobile-first media queries
  * sass-breakpoint plugin
  * window.matchMedia in browser
* Interesting technologies: Sass, Browserify, Grunt, Bower, jBuilder, jQuery UI
  * Use Browserify to make modular front-end code
  * Configure Browserify with Grunt
  * Set up watch with Grunt to re-run Browserify as I work
  * Use Bower for dependency on Marionette
* Deployed to OpenShift
* Test JS with Mocha and Chai
* Backbone.Stickit (two-way model data bindings)
* HTML5 Pushstate enabled in Backbone for friendly URLs
* Backbone views organized based on Pages
* Home view persisted for efficient redisplay
* Backend and frontend support for friendly IDs
* Nested model collections
  * e.g. `Category#children()` and `Category#resources()`
* Pagination
  * See `Resources#url()` and `Resources#fetchNextPage()`
* JS Mixins
  * Showing errors
  * Favorite controls
* `CompoundView`
  * Custom extension of Backbone.View for managing subviews
  * Supports adding, removing, and rendering subviews
  * Each subview assigned to a selector
* `TransitionHelper`
  * Can set transform on element with vendor prefixes
  * Provides `onTransitionEnd` for polyfilling `transitionend` event
* Category List Animation
  * Add new list, setting translate to move next to current and out of view
  * Set transition on translate in CSS
  * Change translates to move old list off screen and new list in
  * Utilize TransitionHelper
* Home view hierarchy
  * HomePage < CompoundView
    * IndexControlBar < CompoundView
      * ControlBar < ItemView
    * CategoryBrowser < CompoundView
      * CategoriesList < ItemView
      * ResourcesList < CollectionView
        * ResourceTeaser < ItemView
* Bootstrap Category data on home page

## Zasteroids

* Use Grunt for build
  * Concat, minify, and version scripts
* Everything in `<canvas>`
* Object Model:
  * MovingObject
  * Asteroid < MovingObject
  * Bullet < MovingObject
  * Ship < MovingObject
  * Game
* Drawing Zombie
  * Save canvas context
  * Translate canvas to x and y position
  * Rotate canvas to `Math.PI * 1.5 + currentDirection`
  * Draw image
  * Restore canvas context
* Set random Asteroid position and velocity
  * Randomly multiply by 1 or -1
* Position and velocity both represented by `[x, y]` arrays
* Math
  * `Asteroid#updateDirection`: Rotate zombie to face ship
    * `opposite = shipY - zombieY`
    * `adjacent = shipX - zombieX`
    * `currentDirection = Math.atan2(opposite, adjacent)`
      * Returns counterclockwise angle in radians
  * `currentFps`
    * `fps = 1000 / (currentTime - previousTime)`
      * `currentTime - previousTime` is time since last frame
    * Pass to `#move` of asteroids, bullets, and ship
      * `MovingObject#move` divides velocityX and velocityY by currentFps
      * Implements **Time-Based Animation**
  * `newPositionX = positionX + velocityX`
  * `newPositionY = positionY + velocityY`
  * `MovingObject#findDistance`: Uses distance formula
    * `Math.sqrt(Math.pow((y2 - y1), 2) + Math.pow((x2 - x1), 2));`
  * `MovingObject#isCollidedWith`:
    * Checks if distance between is less than combination of radii
  * `Ship#draw`
    * Use context to save, translate, rotate, draw, and restore
    * `angle = currentDirection * (Math.PI / 180) + (Math.PI * 0.5)`
      * currentDirection is in degrees
      * Multiplying by `Math.PI / 180` converts to radians
  * `Ship#updateVelocity`
    * `velocityX = Math.cos(degrees * (Math.PI / 180)) * speed`
    * `velocityY = Math.sin(degrees * (Math.PI / 180)) * speed`
    * `degrees` is current direction
* Object pools: `AsteroidPool` and `BulletPool`
  * `#allocate`, `#create`, and `#free`
  * Object freed when going off screen or destroyed

## Protomatter.js
