# Dependency Injection & Controller Integration, Part 2

This lesson focuses on adding create, update, and delete functionality for your `PostsController`. You may have already added some or all of that functionality, but once you create relationships between objects, things can get a little complicated. Furthermore, `update` requires special consideration.

## createPost

On one hand, creating a post in `PostsController` is pretty simple. You just need to use your `postsRepository` to `save` the incoming `Post` parameter. Any fields not provided will take `null` values and the database will automatically generate its primary key value.

However, now that posts have an `author` instance variable of type `User`, you should automatically set the new post's `author` appropriately. 
1. First, fetch a `User` object for whichever user you wish to associate as the author. In the future, we will set the `author` as the user logged into the front-end application. For now, let's just assign the author as the user with id 1.
2. Set the new post's `author` to that `User` object you fetched in #1 above.
3. Then save the post via the `postsRepository`.

Now... you are most likely asking "how in the world can I fetch a user in the `PostsController`??? This is for posts, not users!!!" 
Well, just use Dependency Injection to inject a `UsersRepository` into your `PostsController` in the same way you injected a `PostsRepository`. In fact, if you are using Lombok's fabulous `@AllArgsConstructor`, all you need to do is declare a `UserRepository` instance variable. And voila! Your `PostsController` can now work with `User` data. /golfclap

### Exercise

- Modify your `PostsController` to save a new post with a pre-determined author of your choice (i.e., an id for a user record that is already in your database).
- and modify `UsersController` `createUser` so that it creates a new User. Test it with Postman AND the Register screen in your frontend.

## updatePost

Just like `createPost`, `updatePost` seems simple on the surface. But there is pure evil beyond that facade.

The problem lies in the fact that the request may contain an incomplete post object for updating the database. Hibernate WILL update all fields, missing or not, and if the field is missing when the `Post` object is created by Spring, it will have a value of `null`. This means Hibernate will overwrite perfectly good data in your table with `null`s. So uncool.

While JPA might have some awesome annotation for avoiding this mess, we can instead compare the incoming, possibly incomplete object with a saved copy of the object from the database. Any non-null value in the incoming object should be considered as changed data and can overwrite the same field in the object fetched from the database. Any `null` value in the incoming object should be ignored. Here is the algorithm:
1. Fetch a `Post` object using the `UsersRepository` with the same `id` as the incoming `Post` object.
2. Check each field in the incoming `Post` object. If non-null, then overwrite the same field in the `Post` object from the database with the value from the incoming `Post`.
3. Then save the post via the `postsRepository`.

**IMPORTANT:** We have a SWEET and VERY REUSABLE method you can use to make this a snap! Be sure to ask for a demonstration of it!!!

### Exercise

- Modify your `PostsController` so that `updatePost` no longer obliterates your database records with `null`s.
- and modify your `UsersController` `updateUser` method in a similar fashion.

## deletePost

This one is easy. `PostsRepository` has a perfect delete method to accomplish this and you probably already did this in DI-Integration part 1.

## updatePassword

The algorithm for this method is not bad.
1. Fetch the `User` object from the database whose `id` matches the `id` parameter
2. Set the object's `password` field to `newPassword`
3. Then save the user via the `UsersRepository`.

### Exercise

- Get your `deletePost` and `updatePassword` methods working.

### Extra BONUS Exercise!!!

Modify your `createPost` so that new posts are automatically assigned a few default categories. Yes... this will involved create a 3rd repository instance variable in your `PostsController`. Not a big deal if you are using Lombok.

NOTE: If you went above and beyond in the Building Relationships section, you may already be sending selected categories for the new post in the request body. Use those instead of default categories for the new post.


Congratulations! You have made your `PostsController` officially `SUPER AWESOME`!

## Next Up: [Data Persistence, Part 3](14-data-persistence-iii.md)

