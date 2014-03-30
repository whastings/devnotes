# SQL & ActiveRecord Curriculum Notes - Will Hastings

## Week 3 - Day 1
QUESTION: Does setting a varchar column to a length of only the most necessary make the database more efficient?
- Answer: The db stores data in fixed-width portions in files, so unused characters will be filled with blanks. So some
- Default is usually 255, so you may more often run into the situation where you have to specify it to go bigger
- Only really helps when you know the values or max size of values
    + e.g. Phone numbers
QUESTION: How do relational DBs store boolean values?

**Common SQL Datatypes**:
- BOOLEAN
- INT
- FLOAT (stores "floating point" numbers)
- VARCHAR(255) (essentialy a string with a length limit of 255 chars)
- TEXT (a string of unlimited length)
- DATE
- DATETIMETIME
- BLOB (non-textual, binary data; e.g., an image)

**Foreign Key**: Value of a table entry that points to an entry in another table via the value of its *primary key*

Good style for writing SQL: Put each clause (`SELECT`, `FROM`, `WHERE`) on its own line
- Ex:
```
SELECT
  col_1,
  col_2
FROM
  table_name
WHERE
  condition
```

**Rails SQl Conventions:**
- Name tables in `snake_case` and pluralized
- Name foreign keys as `other_table_singular_id` (e.g. `user_id`)

`GROUP BY` groups result rows together by common values
- Useful when counting things
- Result set will contain one row for each distinct value of grouping column
`HAVING` lets you filter `GROUP BY` data like `WHERE` does on raw data
- e.g. `...HAVING employee_count > 2...`
- `GROUP BY` runs after the where clause, making `HAVING` necessary
- Can also group by expressions (e.g. `GROUP BY SUM(...)`)
- You can group by *multiple columns*
    + It will return a separate group row for each *combination* of the group by columns

In addition to columns, the `SELECT` clause can take literals, expressions (e.g. `column_name * 2`), and calls to built-in functions (e.g. `ROUND`)
With `SELECT`, you can add an alias to columns (e.g. `UPPER(first_name) FIRST_NAME`) by specifying it after the column name
- Will be the column headers in the result set
- You can place `AS` between column name and alias

**Subqueries**:
- Must be in parentheses inside a *containing statement*
- You can provide a subquery to `FROM` and it'll operate on the *temporary table* the subquery generates
- Can also be used in `WHERE` conditions
    + e.g. `WHERE column IN (SELECT...)`
- They have **statement scope**: they're run during the containing statement, then discarded after the containing statement has returned.
- Types:
    + **Non-correlated Subquery**: One that does not use any column values from its containing subquery
        * Can be executed without its containing statement
    + **Correlated Subquery**: One that incorporates column values from its containing statement
        * Is run multiple times for each result of its containing statement
            - So cannot be executed without its containing statement
- **Scalar Subquery**: One that only returns a single row
    + Can be compared to a value in containing statement with operators that take single operands
        * Including: `=`, `<>`, `>`, `<`, etc.
- The **All Operator** (`ALL`) allows you to compare a value to every value a subquery returns and returns true if the comparison is true for *all* returned values
    + e.g. `...WHERE amount > ALL (SELECT...)`
- The **Any Operator** (`ANY`) functions like `ALL` but returns true if the comparison is true for *any* of the subquery results
- The **Exists Operator** (`EXISTS`) works with *correlated subqueries* and returns true if the subquery returns one or more rows
    + e.g. Checking if a particular user has any posts
- You can compare multiple columns in a statement against multiple columns in a subquery when both column groups are in parentheses
    + e.g. `WHERE (amount, tip) IN SELECT (col1, col2)...`
- You can pass a subquery to `FROM` to generate a virtual table.
    + e.g. Joining to the results of the subquery
    + But the subquery must be *non-correlated*

**Views**: A stored query that acts like a *virtual table*
- You can issue queries against the key, and it'll combine with it to form a final query
    + e.g. Running a `SELECT` against the view name
- Syntax: `CREATE VIEW view_name AS SELECT...`
You can provide an alias for a table name in `FROM` by specifying it after the table name (e.g. `FROM node n`)
You can `ORDER BY` expression results in addition to column values

**Operators**
SQL uses `AND` and `OR` instead of `&&` and `||`
SQL uses `=` instead of `==`
SQL has `<>` as a synonym for `!=`
SQL uses `NOT` for `!`
`IS NULL` tests if a column or expression equals null
- Note: `value = NULL` will never be true
`IS NOT NULL` returns true if a column isn't null
`LIKE` compares a value to a simple matching expression that uses `%`
`IN` checks if a value is included in a set of values (which can be from a subquery)
- e.g. `...WHERE column IN ('val1', 'val2',... 'vanN')...`
`BETWEEN` checks if a value falls within a range
- e.g. `...WHERE column BETWEEN 0 AND 10...`
- The upper and lower limits are *inclusive*
`NOT IN` is the reverse of `IN`
SQL supports mathemetic operators `+`, `-`, `*`, and `/`
`REGEXP` tries to match a column or expression to a regular expression
- Does not use enclosing `/.../`
- PostGres uses `SIMILAR TO`
    + QUESTION: How do I use it?

**JOINs**:
- A `JOIN` without an `ON` condition is called a **Cartesian Product** or **Cross Join**
    + It gives you every possible *permutation* of the two tables
    + Rarely used
- The default type of a `JOIN` with an `ON` is `INNER`
- If the joining column has the same name in both tables, you can use `USING (column_name)`
- You can `JOIN` to a subquery result
- You can join a table multiple times to different tables
    + The table joined multiple times with need a different alias for each join
- **Self-Join**: Joining a table to itself
    + **Self-Referencing Foreign Key**: Foreign key that refers to another row in the same table as the referring row
    + The table needs a different alias for each join
    + e.g. Joining non-supervisor employees to their supervisors
- You can join tables with operators other than equals, including `>`, `<`, and `<>`
    + e.g. `...FROM table1 INNER JOIN table2 ON table1.date >= table2.date`
    + Can join tables without foreign key relationships
You can even switch `ON` conditions and `WHERE` conditions, or put them all in `ON` or `WHERE`
An **Outer Join** includes all the rows from one table and the matching rows from the other table *if available*, otherwise filling in `NULL`
- `LEFT OUTER JOIN` includes all rows from the first table
- `RIGHT OUTER JOIN` includes all rows form the second table
- A **Natural Join** is one that lets the DB server infer the join conditions so you don't have to specify them
    + Not a good thing to use.

**Aggregate Functions**:
- Common ones: Max(), Min(), Avg(), Sum(), Count()
- If you select only the results of aggregate functions, you'll have an *implicit grouping* (one row in result set with data based on all rows analyzed)
    + If you select a column that's not an aggregate function, you must specify a `GROUP BY` so you get an *explicit grouping*
- If you want to count only *distinct values* in a column (ignore) duplicates, use `COUNT(DISTINCT column)`
- Can also be applied to expression results
- Most aggregate functions skip `NULL` values, though `COUNT(*)` will count it as 1

## Week 3 - Day 2
QUESTION: How does a rollup work?
`UNION ALL` can merge the results of two separate queries

**Indexes**:
- They're special tables that are kept in a *specific order*
    + Regular tables aren't kept ordered
    + Contain just the column(s) used in the index and a pointer to the corresponding row's location
- An index for a table's primary key is automatically added on table creation
- You can create an **unique index** for force a column's value to always be unique
    + This isn't necessary for the primary key column
- A **multi-column index** keeps data sorted by the first column, then the second, and so on
    + If you have a two-column index, you can search by both or the first column, but *not* by the second
    + e.g. On last_name, first_name
- Index type tends to default to **B-Tree (Balanced Search Tree)**
    + Holds data in leaves
    + Uses branch node values for tree navigation
    + Tries to stay balanced (same amount of branches and leaves on each side of the root node)
- A **Bitmap Index** is good for columns with a small number of possible values used across many rows
- Downside: The more indexes you have, the more that have to be updated as records are inserted, updated, and deleted
- Things to index:
    + Columns that are used for lookups (e.g. foreign keys)
    + Columns that are used for sorting

You can use the `EXPLAIN` keyword before any query to get more info on how it'll run

**Constraint**: A restriction placed on one or more columns
- Includes primary key, uniquess, and allowable values
- **Foreign Key Constraint**: Allows a column to only contain values from another table's primary keys
    + Server will alert you if you try to delete or modify a primary key value that's referenced from other tables

**Heredocs**: Ruby has two ways to start one: `<<` and `<<-`
- The second form allows the ending delimiter to be tabbed in
- Convention is to have the delimiter string be the language being embedded
    + e.g. `SQL` or `JSON`

**SQLite**
- Stores DB in a simple file
- Ruby uses it through the sqlite3-ruby gem.
    + Can connect to a db by instantiating or subclassing `SQLite3::Database`
    + Returns results as arrays by default
        * Can change to hashes with `self.results_as_hash = true`
            - Are string-indexed
- Has meta-commands to show info in CLI.
    + `.tables` shows DBs tables.
    + `.schema` shows create queries for tables.
- Supports basic [data types](http://sqlite.org/datatype3.html)
- To set up foreign key constraint: `column INTEGER REFERENCES other_table(id)`
- You can run a sql file against a db with  `cat file.sql | sqlite3 file.db`
- Run queries with `SQLite3::Database#execute`
    + Supports numeric parameters with `?` and named with `:name`
        * Can pass numeric replacements as additional arguments
        * Can pass named replacements as additional hash argument

Ruby's **Singleton Module** makes it easy to have a class be a singleton
- Gives you an `instance` method through which you can access the single instance
    + e.g. `MyClass.instance`
- Makes the new method private

## Week 3 - Day 3
**Explicit Associations**: Rails can infer class and column names, but you can also specify them to the association setup call via a hash
- `class_name` specifies the class of association objects
    + e.g. `class_name: "User"`
    + Without, Rails will infer singular, Pascal case (e.g. `User`)
- `foreign_key` specifies the column name that points to an object's "owner"
    + Is the column on the table for the class that calls `belongs_to`
    + e.g. `foreign_key: :user_id`
    + Without, Rails will infer singular + `_id` (e.g. `user_id`)
- `primary_key` specifies the column name that holds the primary key of the "owner"
    + Is the primary key column name on the table for the class that calls `has_many`, `has_one`, etc.
    + e.g. `primary_key: :id`
        * Is usually `:id`, which is what Rails will infer w/o it
- `primary_key` and `foreign_key` are the same on either side of `has_many` and `belongs_to`
**N + 1 Query Problem**: When you fetch a list through one query, then do a new query for every row returned for the first query
- *Very inefficient!*

**The has_many :through association**: 
- Typically relates two models in a *many-to-many* association via a **join table** 
- e.g. Doctors and Appointments that get to each other through an Appointments model/table
    + `has_many :patients, through: :appointments, source: :patient`
- `:through` indicates the association that will link to the new association
- `:source` indicates the association on the through association that you want to end up at
- So the association travels first through `:through`, then `:source`
- It's common for two models to have `has_many through` associations to each other
- These utilize SQL `JOIN`s
- Can be used to build a new association from any two associations
    + e.g. two has_many associations
- Gets the `:primary_key`s, `:foreign_key`s, an `:class_name`s from the existing associations it links

The **has_and_belongs_to_many association** defines a many-to-many association without a third model in the middle
- Still requires a join table, though

**has_one** works just like `has_many`, but will return just one object instead of an array with one or more objects

**Reflexive Association**: One that refers to the same model/table
- e.g. An employee model in which some are managers and some have managers

**Validations**:
You can apply a *presence validation* to an association.
You can create and run a custom method for a validation by calling `validate :custom_method_name`
- If validation is invalid, indicate this by adding a message to `errors`
You can also create a custom validation class by having it extend `ActiveModel::EachValidator` and implement `validate_each`
- Method takes record, attribute name, and value arguments
You can pass a validation a `:message` option to specify a custom error message
Can make a validation conditional by specifying `:if` or `:unless`
Unless explicitly checking the boolean result of `save`, call `save!` so an exception will alert you if something goes wrong

**Migrations**:
You can define database modifications in a `change` method
- If you're making a modification that ActiveRecord won't be able to infer who to reverse, you'll need to do it in an `up` method and specify the reversal procedure in a `down` method
The `db/schema.rb` file contains the canonical description of your database structure
`rake db:create` will create your database for you
`rails generate migration` will create a migration file/class for you
Database types:
- String will give you a varchar
- Text will give you unlimited-size text field

**config/database.yml** stores dev, test, and prod database config
- To use PostGres, specify `adapter: postgresql` and have `gem pg` in your `Gemfile`

**Rails Console**
- Can reload all classes with `reload!`
- Open with `rails console`
- And you can get to your db console with `rails dbconsole`

## Week 3 - Day 4
**includes and joins**:
- `includes` will *eager load* objects from an association in a single query, preventing N + 1 queries
    + e.g. Loading a user and all her posts
        * `User.find_by(user_id).includes(:posts)`
            - Does one query for user and one more for all user's posts
- `joins` will join a query to load an object to one of its associations
    + *Does not load the associated objects*
        * You need `includes` to load the objects
    + Does an `INNER JOIN` by default, so is useful for limiting records
    + e.g. Load only users who have posts
        * `User.joins(:posts).uniq # Use unique to filter duplicate results.`
    + To do an outer join, you must pass a SQL fragment
        * e.q. `User.joins("LEFT OUTER JOIN posts ON users.id = posts.user_id")`
            - Would also select users without posts
    + You can perform a series of joins by passing `joins` a hash
        * e.g. `User.joins(posts: { comments: :likes })`
            - Will inner join users to posts to comments to likes
- `references` adds an `include`d association from a `joins` association using a `JOIN` instead of a separate query
    + e.g. `User.includes(:posts).references(:posts)`
        * Will do a `LEFT OUTER JOIN`
    + Alternatively, you can use the `eager_load` method

**ActiveRecord::Relation**: The object type returned by query methods like `where`
- Is *lazily evaluated*: runs the query when you first attempt to use the data, not when it's created
- Employs *caching* on the query results
    + Using the same relation's data twice won't run the query again
    + You can pass the method that generated the relation `true` to bypass cache
- You can chain relations together to build new relations
- Can be accessed like an array (e.g. with `each`)
- In Rails 4, associations return a special `CollectionProxy` object that will become a relation when you call methods like `where`

The `select` method allows you to add arbitrary data to your loaded objects
- e.g. `User.select("users.*, COUNT(*) as posts_count")...`
    + Will add a `posts_count` method to the loaded objects

QUESTION: For scopes, should we use class methods or the `scope` keyword

**Querying Helpers**:
- `order` takes SQL fragment specifying column(s) to order on
- `group` takes SQL fragment specifying column(s) to group by
- `having` takes SQL fragment specifying having conditions
- There's also aggregations: `count`, `sum`, `average`, `minimum`, `maximum`
- `find_by_sql` lets you provide raw SQL for finding records

You can create objects through associations
- `Post.comments.create!(...)`

With a `has_many`, you can:
- Do `object.association << new_object`
    + Automatically sets `new_object`'s `belongs_to` id
- Do `object.association.create!(new_object_options)`

You can specify custom criteria for a `distinct.count` with `select`
- e.g. `object.association.select(:column).distinct.count`

**Named Scope:**
- Creates a method for returning a query relation
- e.g. `scope :recent -> { where('created_at > ?', 1.hour.ago) }`

The `pluck` method on a model or association will return an array with the values for the `pluck`d columns
- e.g. `User.posts.pluck(:title)`

## Week 3 - Day 5
**Metaprogramming**:
- `Object#send` lets you call a method by name (string or symbol) dynamically, passing any additional arguments on to it
- `Module#define_method` lets you define methods dynamically
    + Ex:
```
class Something
  define_method(:a_method) do |an_argument|
    puts an_argument
  end
end
```
- `Object#define_singleton_method` is like `define_method`, but adds the method to the object's singleton class
    + Good for defining *class methods*

**Class instance variable**: Variable shared for all instances of the same class
- Still prefixed with `@`, but declared with `self` is the class object
- But aren't inherted to subclasses

With modules, mixing in with `include` will mix in all methods as *instance methods*, `extend` will add them as *class methods*
