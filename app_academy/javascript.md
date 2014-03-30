# JavaScript Curriculum Notes - Will Hastings

## Week 6 - Day 1
QUESTION: Does JS's prototypal inheritance give it performance advantages over classical scripted languages?

**JS's Primitive Types**: strings, numbers, booleans, 'undefined' and 'null'

**JS's Falsey Values**: 'undefined', 'null', 'NaN' ("not a number"), 0, and '' (empty string)

Use `_variableName` to indicate a variable that should be treated as private

**call() and apply()**:
- Use `call` to pass `this` argument and additional arguments as a variable list.
- Use `apply` to pass `this` argument and additional arguments as an array

Can make multiline strings by ending each line with `\`

**Constructor Inheritance**:
- Call "superclass's" constructor from "subclass's" constructor
    + `SuperClassConstructor.call(this, arg1,... argN);`
        * Will add any "superclass" instance variables to the "subclass" instance
- Use `Object.create` to make "subclass" constructor's prototype inherit from "superclass" constructor's prototype
    + `SubClassConstructor.prototype = Object.create(SuperClassConstructor.prototype);`

Generating **Random Numbers**:
- e.g. `Math.floor(Math.random() * 2);` gives either 0 or 1

**Great ES5 Array methods**: `Array.isArray`, `every`, `filter`, `forEach`, `map`, `reduce`, `some`

Use **parseInt** function to convert strings to numbers

In JS, you need to call `return` explicitly, or else function will return `undefined`

Type checking with **typeof**:
- Syntax: `typeof object === 'typeName'`

Functions don't care if you pass too few or too many arguments
- Extra are ignored; ommitted are set to `undefined`

## Week 6 - Day 2
Use Node's **readline** library to take console input.

Ways to set **this**: 
 - Method call style: `this` set to receiver object
     + e.g. `myObject.myMethod(); // this will be myObject in myMethod.`
 - Functional call style: `this` set to global object (or undefined in strict mode).
     + e.g. `myFunction();`
 - Constructor call: `this` set to a new, empty object
     + e.g. `var newObject = new MyConstructor();`
 - **Function.prototype.bind**: `this` set to the argument to bind
     + e.g. `myFunction.bind(myObject);`
 - `call` and `apply`: `this` set to first argument
     + e.g. `myFunction.apply(myObject, [arg1, arg2,... argN]);`
     + e.g. `myFunction.call(myObject, arg1, arg2,... argN);`

`this` can't be captured by closures
- You can alias it in the outer scope (e.g. `var self = this;`)

The basic **Module Pattern**:
- Ex:
```
(function (root) {
  var module = root.module = (root.module || {});

  var function1 = module.function1 = function() {
    // do work...
  };

})(globalObject);
```
- Ex:
```
globalObject.module = globalObject.module || (function() {
  var function1 = function() {
    // do work...
  };

  return {
    method1: function1
  };
})();
```

You can put a plus before a variable as a short-hand for parseInt
- e.g. `+numberInput;`

You can use **Array.prototype.some** to loop through all elements in an array and break early (by returning true)
- Use this instead of `forEach`, as it *will not let you return early*
- Ex:
```
array.some(function() {
  // ...
  if (condition) {
    return true;
  }
});
```

## Week 6 - Day 3:
The **arguments** object:
- Can iterate through it with a for loop
- Can create a true array copy of it:
    + `var args = Array.prototype.slice.call(arguments);`

**Currying**: Creating a function that wraps another function, passing it one or more arguments as fixed values and accepting any other arguments it needs as its own
- Can be done with `Function.prototype.bind`, as it takes fixed arguments to pass along in addition to the `this` argument

With *Underscore*, you can either pass the object to `_.method` or use underscore to wrap it first:
- `_(object).method();`

## Week 6 - Day 4:
QUESTION: In asteroids solution, what's all the JSON stuff about?
QUESTION: In asteroids solution, why are you tracking time ticks in Game?

**CSS**:
- You can't specify height and width on inline elements.
    + But you can set it on `inline-block` elements

**jQuery**
- Can test if a selector found any elements with the `length` property
    + e.g. `if ($('element')) { ...`
- Can grab one element out of a selection by index using `eq`
    + e.g. `var $firstItem = $('selector').eq(0);`
- Can test a selection using `is` (returns true or false)
    + e.g. `if ($('selector').is('.a-class')) { ...`
        * If items have a class.
- The **event** object passed to event handlers gives event info.
    + `event.currentTarget` gives the element that has the event handler attached.
    + `event.target` gives the element that originally triggered the event
    + Can prevent default action (e.g. link follow) with `event.preventDefault()`
    + Can stop bubbling with `event.stopPropagation()`
- Doing **Event Delegation**
    + DOM events bubble, so you can catch a child element's event on a parent
    + e.g. `$('parent-selector').on('event', 'target-selector', handler);`
- `event.preventDefault()` 
- Constructing an element
    + e.g. `var $listItem = $('<li></li>');`
    + Can add to DOM with methods like `after`, `before`, `append`, `prepend`, etc.
- Uses **implicit iteration**
    + Setters will apply to *all elements* in the selection
    + But getters will only return value for the *first element*
- Has methods for getting to nearby elements in the DOM
    + `parent`, `children`, `next`, `first`, `find`, etc.
- Can change classes: `addClass`, `removeClass`, `toggleClass`
- Can iterate explicitly with `each`
    + Passes callback index and raw DOM element
- Can get and set form input values with `val`
- Can get and set properties like `checked` and `selected` with `prop`
    + e.g. `$( 'input[type="checkbox"]' ).prop( 'checked', 'checked' );`
- Can get and set arbitrary attributes with `attr`
- Can clone an element with `clone`
- Can remove an event handler with `off`
- Can use `detach` to remove an element from the DOM without losing any event handlers attached to it.
    + Useful if you want to make a lot of DOM changes within the element with better performance

# Week 6 - Day 5
**Ajax**:
- Implemented in browser as the **XMLHttpRequest** object
- jQuery gives you `$.ajax`
    + Takes `success` and `error` callbacks in options object
    + Specify HTTP method with `type` option
    + Specify type of data to receive (e.g. JSON) with `dataType`
    + Set data to send with the `data` option
    + Returns a **jqXHR** object
        * Is a promise, providing `then`, `done`, and `fail`
- Can only make AJAX request to same domain
- **JSONP**:
    + Inserts a script tag to load data from a different domain
    + Can make such a request with `$.ajax` by setting `dataType` to `jsonp`
- jQuery's **serialize** when called on a form element will extract form data to url-encoded string for ajax sending
    + Can get it serialized to JSON with the **serializeJSON** jQuery plugin
        * Makes it easier to send nested form data.

**Underscore templates:**
- Parse a template into a template function with `_.template(templateContents);`
    + Can pass variable values to generated function
        * Pass as object literals with variable names as keys
        * Variables will be available in templates by name
- Can place template contents in script tags with `type="text/template"`
- Since Underscore uses same syntax as ERB, you need to escape starting percent signs if you write a _ template in an ERB file
    + e.g. `<%% objects.forEach(function(object) { %>`

**Data Bootstrapping**: Getting initial data on to the page that will be updated via ajax
- Can do so by having the server write initial data as JSON on the page.
    + Can put in a script tag with `type="application/json"`
- Can print data in Rails view as JSON with `<%= variable.to_json.html_safe %>`
    + But watch out for XSS vulnerability in Rails 3

In Rails, place downloaded JS libraries in `vendor/assets/javascripts`
- Have to list in `application.js` to load it

**Using AJAX with Rails**
- When saving an object, you should render json of saved object if successful
    + Otherwise, render errors as json with error status code
- If you send Rails JSON data, it will automatically parse it into the params hash
- When making a request to Rails, you can specify the content type you want by:
    + Adding an extension to the url (e.g. `.json`)
    + Setting the `Accept` HTTP header

Can flatten a JS array with `Array.prototype.reduce` 

## Week 7 - Day 1
**Backbone.js Basics**:
- **Models**
    + `initialize()` takes options about to set initial attribute values
    + Store data internally that can be accessed and modified with `get()` and `set()` methods
        * Internal data available through the `attributes` property, but use with caution
        * Can escape attribute value with `escape`
    + Holds database ID in `id` property
        * Backbone uses it to tell whether to issue `POST` or `PUT` on save.
    + `urlRoot` options specifies server url to access for saving/fetching
    + `defaults` property specifies default values for attributes
    + `fetch` loads/updates model from server
        * Can override how the server data is parsed by overriding `parse()` method
    + `save` creates or updates model on server
        * Can override what data is sent to the server by overriding `toJSON`  
        * Takes `success` and `error` callbacks
    + `validate()` method called when attribute values change
        * Return a string to indicate an error.
    + **Model Events**
        * Models fire events when their attribute values change
        * Can subscribe to them with `model.on('event', callback);`
        * Fire an event with the `trigger()` method
            - e.g. `this.trigger('an-event');`
        * Include `change`, `change:[attr]`, `destroy`, `sync`, `error`
    + `toJSON` will return POJO representation of model's attributes
    + In Rails, good to store in `app/assets/javascript/models`
- **Views**
    + Are more like Rails controllers than its views
    + `el` and `$el` store the DOM element the view manipulates
        * Will default to an empty div
        * You can set it explicitly or set the `tagName` property
        * Can set element's id with `id` and classes with `className`
        * You can set element attributes with the `attributes` property
    + `template` option takes the template function to run to generate contents of `el`
    + `render()` responsible for adding contents to `el` and adding it to the page
    + Can take `collection` or `model` options to specify the data it represents
    + **Event Handling**
        * `listenTo()` can register callbacks for model or collection events
            - e.g. `this.listenTo(this.collection, "reset add", this.render);`
            - Can list multiple events with same callback using space separated list
                + e.g. `'sync add'`
            - Good for knowing when to re-render
            - `sync` event fires when a collection reloads itself
            - Don't have to bind callback, as Backbone will call with right context
            - Helps you avoid excessive callbacks
        * `events` property maps event types to method names
            - e.g. `events: { 'click .submit-btn': 'submitForm' }`
            - Can handle events on `el` or its children
    + **Rendering**
- **Collections**
    + `model` option specifies the model type stored
    + `url` specifies the model index url to load from
    + `get()` takes ID and finds the model in there.
    + `at()` takes index and retrieves model
    + `models` property holds the model objects
    + `remove()` deletes the given item from the collection
    + `fetch()` retrieves models from server using index path
        * Is async
        * Takes options object with `success` and `error` callbacks
    + `toJSON()` will return a POJO representation of models in an array
    + In Rails, good to store in `app/assets/javascript/collection`
    + Also have *events*
        * Include `add`, `remove`, and `reset`
        * Also fires events that happen on any of its models
    + Has all of Underscore's collection methods
    + Can define how a collection is sorted by setting the `comparator` attribute to an attribute to sort by or a function that takes and compares to elements
        * For functions, can take a `sortBy` (takes one model) or a `sort` (takes two models)
            - `sortBy` should return attribute to sort by
            - `sort` should return -1, 0, or 1
- **Router**
    + Use `Backbone.history.start()` to get Backbone to listen for url changes
        * If you give the option `{pushState: true}`, it'll use the HTML5 history API
    + Takes a `routes` option with path keys and method name options
        * e.g. `{ 'user/:id': 'showUser' }`
        * Make path parts optional by surrounding with parentheses
        * Can also provide regular expression to further limit how a path is matched
    + **Fragment ID**: URL part after the `#`
    + Need to instantiate your router before it takes effect.
    + Can programmatically navigate with **Backbone.history.navigate()**
        * Takes path and options object
        * Options object requires `trigger: true`

- Can give Backbone object properties function values for dynamically calculating property values
    + e.g. Giving `url` a function to compose url with correct ID
- Can give most "classes" an initialize method to set up custom properties and attributes

Underscore's `extend` method copies object attributes from arguments after one to first object passed

**EJS**: JS templating engine
- Can add to Rails with `gem ejs`
- Uses ERB syntax
- Put each template in own file in `app/assets/templates`
    + Need to source this directory by adding line in `config/application.rb`
        * `config.assets.path << 'app/assets/templates'`
    + Then, require templates directory from `application.js`
- Extension is `.jst.ejs`
- Templates available in JS via `JST[templateName]`

Using forms with Backbone
- Use a View class to represent the form and a template to define its HTML
- Listen for the form's `submit` event in your view's `events` and map to a callback
- Create a new model from the form data
    + Can use jQuery.serializeJSON to avoid pulling out form values manually
- Save model and update relevant collection(s)

Can namespace routes in rails with **namespace**
- e.g. `namespace :api do...end`
    + All routes within will have `api/` prefixed
    + Controllers will go in `app/controllers/api` and be within an `API` module
- Can specify defaults with `:defaults` option
    + e.g. `defaults: { format: :json }` to have all responses default to json

## Week 7 - Day 2
In your Backbone Router, make sure each path can load the data it needs
- Can't depend on the user having visited a previous route
- Can create a collection `getOrFetch` method

Representing Associations in Backbone
- Can do so with a method that returns collection
- Can have model listen for and re-fire sync events on its associated collection(s)
- 
**JBuilder**: Helps you build up JSON for responses
- Load with `gem jbuilder`
- Uses JSON templates like ERB does for HTML
    + Go in same view folders as regular views
    + Extension is `.json.jbuilder`
    + List attributes to include with `json.(object, attr1, attr2,... attrN)`
    + Can return a json array from an array with `json.array!(array)`
        * Pass block to tell how to render each element
        * Delegate item rendering to partial with `json.partial!(partialName, valuesHash)`
    + Can also name the association with `json.associationName(array) do...`
- Useful for adding association data to a model response
    + Override model's `parse()` to take association data and put it into model objects in a collection
- To use it, have controller action render template instead of json

## Week 7 - Day 3
Watch of for Backbone **Zombie Views**
- Views can't get garbage collected if models/collections they're listening to still reference they're callbacks
- So use the view **remove()** method
    + It will remove the view's HTML and unbind all its listeners
- Good practice to write a helper method on the router for *swapping* views
    + Store a current view instance variable and call remove on the old one before assigning the new one

## Week 7 - Day 5
Can make your own "superclasses" that sit between your model, view, etc. and the built-in backbone class
- **Composite View** is an example
    + Give it a `subviews` attribute
        * Make sure it goes on the instances, not the prototype
    + Can add methods like `addSubview`, `renderSubviews`
If you remove from the DOM, then re-add a Backbone view instance's `$el`, make sure to call its `delegateEvents` method to reattach any DOM listeners
If you have views with subviews, make sure they call remove of their subviews when they're removed

**Final Project Advice**
- Make something employers can review quickly
- **Log in as guest** is GREAT

## Week 8 - Day 2
**Node.js**:
- **Non-blocking IO**: Main process triggers IO operation then continues on without waiting for it to finish
    + Callbacks used to react to finish of IO operation
- Uses a **Single-threaded Event-Loop**
    + Your Node code only runs in one process
- Handles HTTP requests with **http** module
    + Has **createServer** method that returns a server object
        * Takes a callback to which it will pass the request and response object
    + Can pass a callback to handle server requests with `server.on('request', callback);`
        * Callback will get `request` and `response` arguments

**Long-polling**: Pre-websocket technique to allow server to send data to client, involving sending an Ajax request and keeping it open until the server has something to send

**Socket.io**
- Start listening for connections with `socketio.listen(server)`, which returns an `io` object
- Can handle a connection with `io.sockets.on('connection', callback)`
    + Callback will receive the connection's `socket`
    + Can listen for event's sent from that client with `socket.on(eventName, callback)`
    + Can send event to just that client with `socket.emit`
- Can send an event to all connected clients with `io.sockets.emit`
