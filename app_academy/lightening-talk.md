# Lightening Talks Curriculum Notes - Will Hastings

## Week 8 - Day 3

When writing a Backbone app, a good development order is: model, collection, view, router

When calling a Backbone model's save method, you can pass `{patch: true}` to only send modified attributes

**Figaro Gem**: Let's you store sensitive configuration data in a safe place and makes it available to your app
- Config goes in `config/application.yml`, which you add to `.gitignore`
    + Can make with `rails g figaro:install`
- Makes data available in the `ENV` hash

**jQuery UI**:
- Includes drag-and-drop, sorting
- Can get in rails with `jquery-ui-rails` gem
    + In application.js, can require just the components you need
        * e.g. `//= require jquery.ui.sortable`
- Can handle jQuery UI events with Backbone's event hash
    + Event names are prefixed with the jQuery UI component name
        * e.g. `sortstop`

**Deploying to Heroku**:
Make sure you don't run `rake assets:precompile` (Heroku does that)

## Week 8 - Day 4

**Devise**: Gem for user authentication
- Has a generator to install
    + Gives you further manual setup steps
- Gives you the option to copy its views to your app so you can edit them
- Is a **Rails Engine**: A gem that works like an internal Rails *sub-app*
    + Connects the engine app to the outer app
- Has command for adding to/creating a model: `rails g devise UserModel`
    + If you don't have the model, it will create it
    + Will add a migration to add to or create the model's table

**Omniauth**: Gem for OAuth with devise
- Has various supporting gems, one for each service (e.g. GitHub)
- Requires you to create an `authorizations` model
- Needs the `uuid-tools` gem to work
- Remember to store provider keys in environment variables

## Week 8 - Day 5

**Useful CSS Selectors**:
- `:first-child`: An element that is the first child of its container
- `:last-child`: An element that is the last child of its ancestor
- `~`: Select all elements of a selector after a given selector
    + e.g. `.start ~ .sibling`
- `:nth-child(n)`: Style an element that is the Nth child of its container
- `:nth-child(xn)`: Style every Nth child
    + e.g. `:nth-child(2n)`: every other child

## Week 9 - Day 1

**Paperclip**: Gem to upload photos and other files
- Create migration with `rails g paperclip model photo_attr_name`
- In model class, specify `has_attached_file`
- Can use AWS to store photos.
    + Use the **aws-sdk**

**Filepicker.io**: Service for fancy file upload forms
- Gives you a `filepicker_field` form input helper

## Week 9 - Day 2

**Kaminari**: Gem for paginating

**Infinite scroll**
- Can listen to scroll events with `$(window).on('scroll', callback)`
- Can check window's scroll position with `$(window).scrollTop()`
- Can check if you're within a certain amount of pixels from the bottom:
    + `$(window).scrollTop() > ($(document).height() - $(window).height() - 50)`

Underscore's **throttle** method lets you limit how often a function can run
- Good for limiting an event handler for an event that occurs frequently (e.g. mouse movement)

## Week 9 - Day 3

**Delayed Job**: Gem to schedule work for background processing
- Stores work to do in its own DB table
- Call `handle_asynchronously`, passing it a method name, and it will handle it
- Can start a process to run tasks as they come in with `rake jobs:work`
    + This will continue running and wait for tasks

**Rake Tasks**:
- Can generate with `rails g task namespace name`, then go edit in `lib/tasks`

## Week 9 - Day 4

Web page request steps:
- Check browser and OS cache for DNS lookup
- Do actual DNS lookup if necessary
    + Contacts your configured or assigned DNS server
        * This server will contact others if it doesn't have the record
- If CNAME returned, ask it for A record to get IP
- Make request to IP returned from lookup
- Response returned in TCP packets
    + Usually about 1,500 bytes
        * Set as **Maximum Transfer Unit (MTU)**

CDNs are great because
- They're geographically distributed
- They reduce requests to your app server
- If they're on another domain, the browser won't send your app's cookies to them

Trick: Can see you many DOM nodes you have on your page with:
- `document.getElementsByTagName('*').length`

**Threads vs. Processes**:
- A process is given by the OS with its own memory space
- More than one thread can exist in a single process
- All threads use a shared memory space

**Scaling Databases**:
- Indices!
- Sharding databases
    + Masters/Slaves: Master can handle all writes, replicating them to slaves that handle reading
- **Denormalization**: Duplicating data so that it will be in places that are faster to access
    + e.g. Counter cache columns
- Flat file data stores/NoSQL
    + Best for non-relational data or caching results of relational queries

**Caching in Rails**:
- Redis is great for caching
- **Rails.cache** is the key/value store that proxies to your caching solution
    + Can add entry with `write` method
    + Can retrieve entry with `read` method
- Can cache fragments of a view with `<% cache(cache_key) do %>`
    + Then put the contents to cache inside the block

## Week 9 - Day 5: Design Patterns

MVC
Observer
Singleton
- Some see it as an antipattern
Strategy: Deciding between multiple algorithms to accomplish the same task at runtime
Template: A base superclass defines methods that subclasses need to define
Factory: Convenience methods for setting up states of new objects
Finite State Machines:
- Object can only have one state at a time out of a finite set of states
- Has methods to transition between states

