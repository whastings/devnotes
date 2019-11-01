# GraphQL Summit 2019 Notes

## Keynote

* Data Graph champion
	* Set up tools, workflows, and infra
	* Set patterns and best practices
	* Help new teams get started
* Federated architecture
	* About teams working together to produce a unified graph
	* Gateway handles incoming queries and delegates to services
		* Is a piece of shared infrastructure
* Graph Manager
	* Keeps track of schema and queries in use in production
	* Checks to see what would be broken by a change
* React and Apollo
	* Both declarative tech
	* Hooks are the best API so far
	* Both integrate well with TypeScript
	* Client schema for local state
		* Exposed to tools just like server schema
	* Client v3.0 supports skipping normalized storage for data that doesn't need it (e.g. search results)

## Migrating to Apollo at Airbnb

* Recently rewrote mobile web app as PWA, using GraphQL
* Also adopting on for Desktop, iOS, and Android
* Coming from REST, React, & Redux
* Moving to React + Apollo + TypeScript
* Sometimes rewrite something if timed with redesign or other big product change
* Safest approach is incremental adoption
* Article: Reconciling GraphQL & Thrift at Airbnb on Medium
* Talk: Adopting TypeScript at Scale
* Can use `apollo-link-rest` to make Apollo work with REST endpoints
* Started by moving from manual API requests to manual `apolloClient.query` calls
* Use GraphQL Playground instead of Graphiql
* Use GraphQL aliasing to rename fields as needed (e.g. camelCase to snake_case)
* For now, using adapters to transform GraphQL response to match previous REST response
	* So code outside API calls doesn't need to change
* Use generated GraphQL types throughout rest of their code
* Check out `idx` npm package for handling possible null values until optional chaining comes to JS
* Next, moving from Apollo HOCs to Apollo hooks
* Switching components from getting data from Redux to getting it from Apollo
* Using Query Fragments to spread fields needed to many React components, starting at leaves
	* Top level query in root component, composed from fragments from descendant components
* Going to build service worker query pre-fetching
	* Can be even faster than server-side rendering for initial render
* Working on unified schema
* Using federation to link up multiple services to the graph

## Cache All the Things!

* Using in Trulia
	* For property listing
* Use React, Apollo, TypeScript, and Next.js
* GraphQL server sits in front of 20+ REST APIs
	* Some are even third party
* Use GraphQL Data Source on backend w/ Node.js
* Use caching to reduce number of requests from GraphQL gateway to backend services
	* For data that doesn't have to be realtime
* Varnish to cache HTTP calls from GraphQL Gateway to backend services
* Also cache DNS layer
* In data loader, add batching and in-memory caching

## State Management in GraphQL using React Hooks & Apollo

* Shruti Kapoor - PayPal
* Global state vs local state
	* Global
		* e.g. Current user authorization
	* Local
		* e.g. Forms
* Redux
	* `connect` any component
	* No prop drilling
	* But all data in global store, lots of boilerplate
* Context Provider
	* Also no prop drilling
	* But bad for performance since all components that depend on it re-render on each update
		* Can move closer to where it's used to mitigate somewhat
* Hooks
	* Can also lift state w/ context
		* Can be combined w/ `useReducer` to create redux-like API
	* Less boilerplate for local state
* Demo app: https://codesandbox.io/s/github/shrutikapoor08/graphqlsummit2019/
	* Dispatches result of GraphQL query in `useEffect` to update data in context provider state
	* `useRelatedSongs` custom hook to separate GraphQL query from component
		* Uses `useQuery` internally
* Also using Gatsby at PayPal

## Fine-Tuning Apollo Client Caching for Your Data Graph

* Ben Newman, Lead Architect @ Apollo Client
* Apollo believes in each org having a unified, canonical data graph
	* Even if someday it's not GraphQL or alternatives in addition to GraphQL
* Client app should replicate a subset of the graph
	* With same constraints and relationships
* Adding garbage collection and new API for managing types in the cache to Apollo Client
* Everything now under `@apollo/Client`
	* Including React API
	* To simplify versioning and imports
* New GC uses Tracing Garbage Collection
	* Through `cache.gc()` method
	* Starts at root query and fans out through tree, deleting objects that are no longer part of query results
	* Can opt something out of GC with `cache.retain(id)`
* Can use `cache.extract()` to get JSON representation of cache
* Cache eviction with `cache.evict(id)`
* Declarative cache config API
	* `possibleTypes` for when you want to use union types or interfaces
		* To tell Apollo Client about the relationships
		* Could be generated programmatically from introspection query result
	* Type policies
		* Per-type config for things like what `dataIdFromObject` does
			* Generating primary key for a type
			* Use `keyFields` to pick one or more fields to use as primary key in cache
				* Resulting ID can be a JSON structure to represent combined fields
		* Are enforced as cache is used
		* Can also be code-generated from schema
	* Disabling normalization
	* Field Policies
		* For dealing with fields that take arguments and so might result in multiple values for one field
			* By default, stores one entry for each combination of arguments
			* But some arguments shouldn't be considered as part of the field instance's identity
				* Particularly paginated-related ones
		  * In new API, use `keyArgs` array to specify which args should be used to make keys
		  	* Can also only store one result for the field with `keyArgs: false`
		* Can define `merge` function for custom logic on how new data merged into existing data
			* Can use this instead of `updateQuery` with `fetchMore`
			* And you only have to define it once instead of every place query is made
			* If multiple fields use same pagination scheme, can reuse helper function for `merge`
		* Can define `read` function for custom field read logic
			* Can be used instead of local state resolver functions

## Scaling GraphQL Beyond a Backend for Frontend

* Michelle Garrett, Conde Naste
* BFF = Backend for Frontend API
	* Alternative for one-size-fits-all API that apps share
		* Can become a bottleneck for new features
	* One API per user-experience or client
		* Built and maintained by same team as front-end
	* Multiple BFFs can access shared services or data sources
	* Easier to adapt as product reqs change
* BFF pattern came before GraphQL, and GraphQL solves many of the same challenges
	* But not all GraphQL APIs are designed as BFFs
* BFF is easy say to introduce GraphQL
* Conde Naste uses it for GQ and Vogue websites
	* They're in React
	* GraphQL BFF sits in front of content API and other APIs
* BFFs can suffer from duplication
	* If multiple BFFs serve up same kind of data and so implement it multiple times
	* Can split out parts of the API that many teams want to use (e.g. Conde Naste's Content GraphQL API)
		* Didn't use Federation for now since they have a BFF rather than shared Gateway
		* First pass: Schema Delegation
			* Queries proxied, but schemas not merged
* Hoping to move to GraphQL gateway instead of BFF

## Apollo Office Hours

* Questions:
  * Getting better test failures
  * Organizing mutation code (especially w/ new hooks)
  * Feedback on cache updates

## GraphQL & Caching

* Marc Andre, GitHub
* People frequently claim GraphQL breaks caching
* Can adding caching in your server-side resolvers
* Clients like Apollo can do client-side caching for you
* HTTP caching is what's hard w/ GraphQL
	* Freshness: How long client can keep resource in its cache
		* Usually; `Cache-Control` with `max-age`
	* Validation: Client can ask if it needs to refetch this thing
		* `Last-Modified` and `ETag`
	* These can all actually be applied to GraphQL
		* You just need to use GET requests
			* If query is too big, you can use persisted queries
* GraphQL can sometimes be hard to cache still
	* If you request multiple things in one request, you have to break cache for w/e thing changes most frequently
	* But highly customizable REST APIs also suffer from this
* GraphQL trades off cache-ability for flexibility
* GraphQL works best for authenticated APIs with data that change frequently since it doesn't need as much caching

## How We Scaled GraphQL at The New York Times

* James Lawrie
* NY Times has had GraphQL in production since Q1 2017
* Finatra & Finagle from Twitter for HTTP interface
* Use Fastly CDN for caching
	* Serves web app and GraphQL config
	* Also used to cache GraphQL API requests
* Supports 70 different clients
* Fastly lets you cache GraphQL POST requests with key based on request body
* Use Automatic Persisted Queries for larger queries
	* Client sends hash representing query
	* If server doesn't have query, client resends with full query and hash
		* And server stores for next time
	* Spec developed by Apollo
* Use Kafka for getting data from publishing platform to GraphQL service
* Use fastly surrogate keys to invalidate only parts of a cached GraphQL response
* Remember most CDNs will work best w/ GET requests

## A Treatise on State

* Jed Watson - Thinkmill
* Helped move JIRA to SPA w/ React
* Component = Combo of state + view
	* Are the new primitive (was previously markup)
* Five types of state
	* Local
		* In component (e.g. form input, menu open)
		* Start here, pass to other components as props
	* Shared
		* e.g. Between user edit form and user display in app header
		* When two or more different parts of the UI need to share state
		* e.g. Is modal open
		* "Go here when you know why you need it"
		* Transition from local when two components need to access and/or manipulate same piece of state, but w/o knowing about each other
		* Use Context API to prevent prop drilling
			* Move from props to Context when there are interstitial components that don't need to know about a prop but would have to pass it down otherwise
	* Remote
		* Isn't state you own
			* Source of truth is server
		* You can cache it and derive things from it
		* But have to sync changes back to remote via API
	* Meta
		* "State about state"
		* e.g. Is request loading, animation state
		* "Provided for you; you aren't the source of truth"
		* Have to use API to change it (e.g. re-fire this request)
	* Router
		* What page the user is on
		* Like meta, provided for you
		* Links or history are API to request changes to it
		* Often triggers load of remote state when changed
		* Don't put knowledge of router state in reusable components
			* Have a shared parent take care of it
* "Complex applications thrive on shared mutable state"
* Redux makes no differentiation between the different types of state
	* It's all shared state
	* Have to manually manage API request states
	* Is synchronous by default
* Benefits of GraphQL + Apollo
	* Select the data you want, and don't have to transform it
	* Data requirements co-located w/ components that need them
	* Handles meta state for you automatically
* "Riding the wave of complexity"
	* You don't want complexity to increase at same rate as features
* "Don't solve problems when you can stop having them"
* "Always identify which component owns which piece of state"

## useSubscription: A GraphQL Game Show

* Still send queries and mutations over HTTP
* Subscriptions go over web sockets
* Apollo has a websocket link
* Have to write a `split` function to tell Apollo when to use websocket vs HTTP
* Apollo has `useSubscription` hook
	* In GraphQL, use `subscription` keyword instead of `query`
		* Otherwise, syntax is the same
* On server, need to use something like Redis for pub-sub if you're going to scale

## Digging into the Apollo iOS SDK

* Ellen Shapiro, Apollo
* Works on Apollo's iOS SDK
* Has its own caching layer
* Getting started w/ SDK has traditionally been hard
* Generates code from schema + queries/mutations, providing type safety
	* So you need to set up schema location, queries location
	* Have to set up run script build phase to do it
	* Code generation written in Node.js and TypeScript
		* So iOS devs had to get Node and npm
* Now including node runtime + dependencies in release artifact so devs can skip npm
	* Downloaded by script if needed when installing iOS SDK
* SDK uses Double Optionals
	* Especially for input objects
	* e.g. `Optional<String?>` or `<Optional<Optional<String>>`
		* Inner `Optional` represents field that may be null
		* Just one `Optional` when getting data from server, but double for input types
* Caching
	* `NormalizedCache` protocol
	* Doesn't return partial results, but hits server if some fields are missing
	* `cacheKeyForObject` option can be used to provide custom function for cache key
		* By default, uses `id` + `__typename`
	* By default, caching is in-memory
	* `ApolloSQLite` library lets you store cache in SQLite
* `ApolloWebSocket` library provides subscription support
* Ran iOS user research survey in September
	* Requests for better docs
* Working on better documentation and example tutorial + sample app
* May switch to `Codable` for JSON parsing instead of current custom implementation
* Will start moving code generation to Swift instead of TypeScript
	* Will be easier for iOS community to contribute to
	* Will add `Equatable` and `Hashable` protocol support
	* Will start implementing Fragments as protocols
* Will work to get rid of Double Optionals
* Long term
	* Better checks for SDK working across package managers
	* Better caching
	* More supplementary libraries (e.g. wrappers for `RxSwift`, `PromiseKit`)
	* Could use help from the community

## SDL as an Artifact: Code-First Schemas in TS/JS

* Tim Griesser - Cypress.io
* graph.ql: First library he found that created JS runtime objects from schema SDL string
* Used graphene Python library
	* Uses code-first instead of SDL-first approach
* Also used graphql-ruby
	* Which also takes code-first approach
* SDL-first seems to be mostly the JS community's way
* Facebook dev confirmed SDL not intended to be human-written
* SDL first
	* End up with duplicate declarations between SDL and runtime type in your code
	* Can't use `lexicographicSortSchema` from graphql-js to auto-sort your SDL since it's hand-written
* Writing GraphQL Nexus library for building code-first in JS and TS
	* Has declarative SDL to create schema in JS or TS
	* Can generate types for TS
	* Outputs SDL as an artifact
	* Supports authorization checks

## Building offline first apps with GraphQL & Apollo

* Kiran Abburi, founder of Neostack
* Service worker for code and asset loading w/ offline support
* Will focus on managing data offline
* Building offline apps is hard when GraphQL/Apollo code co-located with components
	* Doesn't work if you're offline when Apollo-wrapped component rendered
* Instead you need to prefetch data you might need to show the user
	* Can do in a large query at the top of your app
		* Do this with `apolloClient.query`, since you don't want to subscribe to changes and have the whole app re-render
		* Can mark fields that can be loaded later with `@defer` directive, and Apollo will prefetch them after initial load
	* And components can get the data they need from cache
* Apollo will still make request to the server if query for field is different from what originally loaded it
	* e.g. `user(1)` data already in cache from `users` query, but Apollo still goes to network
	* Can provide hints to Apollo to fix this using cache redirects that tell it to look in cache for that field (e.g. `user(1)`)
* Keep data from visited routes in cache so previously visited routes can show when offline
* Can use `apollo-cache-persist` to persist cache data into local storage and keep it in-sync with in-memory cache
* For mutations, built abstraction to represent mutations as plain JS objects, and they go in a queue when offline
	* Use optimistic response to update UI immediately
	* Send entire array to server, and it runs the mutations

## From Vuex to Apollo state management: GitLab journey

* Natalia Tepluhina, GitLab, VueJS Core Team
* GitLab uses Vue
* Moved from Vuex to Apollo + Vue
* GitLab is Rails on backend, w/ lots of server-side rendering
	* Include multiple, small Vue apps on the same page
* Vuex is similar to Redux, but state can be mutated directly
	* For API calls, also has boilerplate like Redux for loading, error, etc.
* Advocates using Apollo Cache for local state as well
	* Rather than Vuex + Apollo
* GitLab server-renders data into HTML element data attributes
	* Then JS seeds data from them into Apollo Cache
	* And then components access Apollo w/ GraphQL
* Vuex has getters (like redux selectors)
	* Can add fields to local schema that transform data from the server like you want
		* Need to provide client-side resolver
		* e.g. To pull element of out collection by ID
* Vuex has actions to update state
	* Can do local schema mutations w/ Apollo for this
	* Just need to write mutation resolver on client
* Cons
	* Verbosity of local resolvers
	* Test coverage
	* Missing best practices

## Building a faster checkout experience at PayPal with GraphQL

* Check out: https://github.com/krakenjs/zoid
* Check out: https://github.com/krakenjs/post-robot
* Build payment eligibility API w/ GraphQL (PayPal, Venmo, Credit, etc.)
	* Let's client request just the payment methods they care about
	* Set up parallel execution of resolver functions
		* Usually GraphQL query data resolved serially, breadth-first
	* Scalable for adding more payment methods
	* Wrote memoize function (Node.js) to memoize API requests
	* Set up caching for backend requests where data doesn't change too often
		* Using their redis-like caching in GraphQL server to skip requests to backend services
	* Instrument resolver duration and display with Grafana
* Also using Gatsby and React

## Client-side GraphQL at Scale

* Chris, Shopify
* Shopify uses GraphQL + TS + React
	* e.g. Shopify Web for merchants
* Use Apollo
	* With hooks
* UI library called Polaris
* Put queries and mutations in separate `.graphql` files
* Wrote graphql-typescript-definitions library
	* Creates a `.d.ts` file next to your `.graphql` files
		* Exports a `DocumentNode<YourQueryType>`
* Wrote @shopify/react-graphql + shopify/graphql-tools-web
	* Provides its own wrappers around Apollo's hooks
		* Hook into GraphQL TS types w/o having to provide them as type args, as long as you use graphql-typescript-definitions
* graphql-mini-transforms
	* Alternative webpack loader for `.graphql` files that produces smaller GraphQL ASTs
		* Doesn't repeat runtime code
* For tests, moved from JSON fixture files for mock GraphQL response to their graphql-fixtures and @shopify/graphql-testing libraries
	* `createFiller` takes your schema and gives you fixtures
		* Gives you a `fillGraphQL` function: you give it a query and it returns fixture data
			* You can pass it subset of data you want to customize
				* Either as object or function (which gets called with variables to query or mutation)
	* Has expectation to confirm a particular graphql query was run

