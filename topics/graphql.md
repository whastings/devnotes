# GraphQL Notes

## Types

* GraphQL has built-in types like `String`, `Int`, and `Boolean`
* You can create your own custom types

## Queries

### Arguments

* Objects can take **arguments**
  * Syntax: `objectName(argumentName: argumentValue)`
  * e.g. `book(title: 'The Stand')`

### Aliases

* Use them to request multiple objects of the same type
  * Syntax: `aliasName: objectName`
  * e.g.
    * `theStand: book(title: 'The Stand') {...`
    * `lordOfTheFlies: book(title: 'Lord of the Flies') {...`

### Fragments

* Use them to easily repeat a set of fields
* Syntax: `fragment fragmentName on ObjectType {...`
* Example:

```
{
  theStand: book(title: 'The Stand') {
    ...bookFields
  }
  lordOfTheFlies: book(title: 'Lord of the Flies') {
    ...bookFields
  }
}

fragment bookFields on Book {
  title
  author
}
```

### Variables

* You can define variables your query takes as arguments
* Syntax: `query ($variableName: VariableType)`
* e.g. `query ($bookTitle: String)`
* You can make a variable a required argument with `!`
  * Syntax: `query ($variableName: VariableType!)`
* You can give a variable a default value
  * Syntax: `query ($variableName: Type = defaultValue)`

### Query Names

* You can name a query
* Syntax: `query QueryName {`

### Directives

* **Include directives** let you include a field only if a boolean variable is true
  * Syntax: `fieldName @include(if: $booleanVariable)`

## Mutations

* Used to write data to a GraphQL API
* Syntax:

```
mutation Name($variableName: VariableType!) {
  mutationName(input: { fieldName: $variableName }) {
    objectToReturn {
      fieldToReturn
    }
  }
}
```

## Pagination

* Fields that return lists will often support pagination via arguments
* One common pattern is edges/nodes
  * Results will often be wrapped in an `edges` list with each result in a `node`
    * Each `edge` can supply a `cursor` field, which is the ID of that node
  * It also supports some metadata
    * You can get the `pageInfo` object, which includes `endCursor` and `hasNextPage` fields
    * You can get the `totalCount` field
* Example:

```
query($bookTitle: String!, $startingReviewId: String!) {
  book(title: $bookTitle) {
    reviews(first: 2, after: $startingReviewId) {
      totalCount
      edges {
        node: {
          text
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

## Best Practices

* Don't try to design the API all at once
  * First, just focus at a high level on the types and the relationships between them
    * Not on the types' fields or mutations
  * When starting on mutations, start without worrying about their inputs or outputs
* Don't let your implementation lead you to the wrong API design
  * Don't expose implementation details the client doesn't need to know about
    * e.g. A join table that implements a many-to-many relationship
      * Just have arrays on the two types pointing back to each other
  * "an API operates for a different purpose than an implementation, and frequently at a different
    level of abstraction" ([link][shopify-graphql-design-tutorial])
* REST and GraphQL are different, so don't assume every REST best practice is a GraphQL one too
* Collapse types that are mostly similar into one type
  * It's okay if some instances have fields that don't apply to them, as they can just be empty
  * e.g. Automatic product collection and manual product collection become collection
* Design your API foremost for the business domain, more so than for the UIs
* Only add fields to your schema that have a use case
  * Don't just add them because they are columns on the database table
  * Remember it's easier to add fields, arguments, etc. than it is to remove or change them
* Include an `id` field in most of your major business types
  * The client can use it for retrieving individual objects, mutations, caching, etc.
  * It's commonly represented in a schema as an interface called `Node`
* Array type fields should usually be non-null and have non-null elements
  * You can represent emptiness with an empty array instead of `null`
* Group closely related fields into sub-object types
  * Look for fields that share a common naming prefix
  * It's fine if the sub-object type doesn't really exist in the implementation
* Boolean fields should almost always be non-null
* Whenever adding an array field, determine if it should be paginated
  * It depends on how many elements it could contain
* Always include a related object itself, not its id, in another object's field
  * You don't want the client to have to make multiple round trips
* Use custom scalar types to provide semantic meaning to clients
  * e.g. If you have string fields that contain HTML, define an `HTML` scalar type
* Use enums for fields which can only take a specific set of values
* Implement mutations as granular actions instead of confining them to CRUD
  * If you only have `update`, it'll become massive
  * e.g. A `Post` might have `postUpdate` for changing it's title text, but a separate `postPublish`
    mutation for the more complex task of publishing
  * When managing relationships, prefer separate mutations for adding, removing, reordering, etc.
    * Consider allowing adding or removing multiple objects at once as a convenience
* Name mutations as `objectAction`
  * e.g. `postCreate`
  * Because all mutations live at the same level and we're stuck with alphabetizing for grouping
* Use more permissive types for scalar input values that require complex validation
  * So the client can let the server do the validation in its business logic
  * e.g. `String` instead of `HTML` or `Email`
* Combine repeated groups of input fields into input types so they can be reused
  * e.g. Reused between the create and update mutations for a type
* Reserve top-level query errors for client and server-level errors
  * e.g. For when the client requested a non-existent field
  * For model validation errors, include them in the return payload of the mutation
    * You can create a type for the mutation return payload that can contain an array of errors and
      the data for the mutated object
    * Example:

```graphql
type PostCreatePayload {
  errors: [Error!]!
  post: Post
}
```

[shopify-graphql-design-tutorial]: https://github.com/Shopify/graphql-design-tutorial
