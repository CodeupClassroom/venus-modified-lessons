# Data Persistence, Part 3

This lesson gives us a deeper dive into using `JpaRepository`.

---

## Defining Query Methods

We can define custom queries by following Spring's naming convention for queries. If we ***name*** our method the correct way, and define the correct ***return type***, Spring will automatically fill in the functionality for us:

```java
public interface PostRepository extends JpaRepository<Post, Long> {
    Post findByTitle(String title); // select * from posts where title = ?
    Post findFirstByTitle(String title); // select * from posts where title = ? limit 1
}
```

The naming convention is as follows:

```JAVA
ObjectToReturn findByColumnName(columnValue);
        
e.g.,
Post findByUserId(long userId);
```

---
## Custom Queries
If we need something more fine-tuned, we can use the `@Query` annotation to specify the query that should be run for the method. 

The string we put inside of the `@Query` annotation is HQL, or Hibernate Query Language. It is very similar to, but slightly different from SQL.

By default, Spring Data JPA uses position-based parameter binding, as described in all the preceding examples. This makes query methods a little error-prone when refactoring regarding the parameter position.

To solve this issue, you can use `@Param` to give a method parameter a concrete name and bind the name in the query, as shown in the following example:

```java
public interface PostRepository extends JpaRepository<Post, Long> {
     // The following method is equivalent to the built in `getOne` method, there's no need to create this example
    @Query("from Post a where a.id like ?1")
    Post getPostById(long id);
    
    // The following method shows you how to use named parameters in a HQL custom query:    
    @Query("from Post a where a.title like %:term%")
    List<Post> searchByTitleLike(@Param("term") String term);
}
```
---

### Use the above examples as templates to complete:

## US6-G: fix your `findByUsername` and `findByEmail` methods in `UsersRepository` AND fix your `getPostsByCategory` method in `CategoriesController`

---

## Performance Tuning

Numerous ways to improve performance of your ORM with Hibernate and Spring Data JPA.

Let's take a look at one way!

### `@DynamicUpdate`

Placing this annotation above an entity class will ensure that only fields which were actually updated will be included in the Hibernate-generated SQL command.

Without `@DynamicUpdate` placed above the `Post` class declaration, 
when we run an update to an existing post's record and did not update the `title` or `user` fields, here is what Hibernate outputs:

```
Hibernate: update posts set content=?, title=?, user_id=? where id=?
```

As we can see, Hibernate included updates to fields which we did not update. This can drastically affect performance in large-scale databases!

Now, try this:

```JAVA
@Entity
@Table(name="posts")
@DynamicUpdate // added this annotation
public class Post {
    ...
}
```

Sending a `PUT` request again to update only `Post.content` will now result in:

```
Hibernate: update posts set content=? where id=?
```

!!! 🧐 `@DynamicUpdate` Need-to-Know

    It is ***very*** important to understand that @DynamicUpdate does *not* discern updated values from null and empty values.
    
    This means if you pass a `null` value for content, you will end up with a `null` column for that `Post` record.

## Next Up: [Code-First Database Design](15-code-first.md)
