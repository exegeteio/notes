# lang/ruby.md

## Rails

### Delegated Types

Delegated types are a way to have a polymorphic relationship between two
models, to inherit shared columns, but also add on additional columns.  For
example, a a blog may have `Post` and `Comment`, both are kinds of `Entry`.
In order to view all activity by a `User`, one would traditionally need to
parse through two different tables.  However, with Delegated Types, one can
query the `Entry` table and retrieve both `Post` and `Comment` entries,
paginate, and display.

Because `Post` and `Comment` are distinct classes, the `render(@entry)`
syntax should allow the two to be displayed independently as well.
