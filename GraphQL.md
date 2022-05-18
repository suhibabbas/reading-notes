# GraphQL @connection

The most popular method for pagination in GraphQL are GraphQL connections.

connections use opaque vocabulary like "cursor", "connection", amd "edge".

## Data Modeling

Amplify automaticlly creates database tables for GraphQL, you can create relations between the data models via the (@hasOne, @hasMany, @belongs, and @manyToMany) directives.

| Relationship  | Description   |
| :------------ |:-------------:|
|  @hasOne   | Create a one-directional one-to-one relationship between two models. For example, a Project "has one" Team. This allows you to query the team from the project record. |
|  @hasMany   | Create a one-directional one-to-many relationship between two models. For example, a Post has many comments. This allows you to query all the comments from the post record. |
|  @belongsTo  | Use a "belongs to" relationship to make a "has one" or "has many" relationship bi-directional. For example, a Project has one Team and a Team belongs to a Project. This allows you to query the team from the project record and vice versa. |
| @manyToMany   | Configures a "join table" between two models to facilitate a many-to-many relationship. For example, a Blog has many Tags and a Tag has many Blogs. |

```
The @hasOne and @hasMany directives do not support referencing a model which then references the initial model via @hasOne or @hasMany if DataStore is enabled.
```
