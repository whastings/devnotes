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