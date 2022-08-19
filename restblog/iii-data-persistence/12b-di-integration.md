# Dependency Injection & Controller Integration

Dependency injection means that we will ***inject*** a class' ***dependencies*** instead of instantiating them manually.

Behind the scenes, Spring (and many other frameworks) create what is called a **container** to store instances of our objects which can be called upon whenever we need without having to use the `new` keyword.

Using DI (dependency injection) can be done as simply as follows:

```java
public class PostsController {
    // ...
    private final PostsRepository postRepository;
    
    public PostsController(PostsRepository postRepository) {
        this.postRepository = postRepository;
    }
    // ...
}
```

We can use DI in most of the classes in our Spring application. We can even inject services into other services! 

This is how you can use it in order to get the list of all Posts.

```java
import com.example.restblog.data.PostRepository;

public class PostsController {

    // These two next steps are often called dependency injection. 
    // 1. We create an instance VARIABLE to receive a Repository instance and 
    // 2. Instruct Spring to pass it to the controller class constructor as a parameter.

    // Spring automatically creates the repository instance
    // and INJECTS it into the controller via its constructor. NEAT!
    private final PostsRepository postRepository;

    public PostsController(PostsRepository postRepository) {
        this.postRepository = postRepository;
    }

    @GetMapping
    public List<Post> getPosts() {
        // because the repository is an instance variable, any of our controller methods can use it!
        return postRepository.findAll();
    }

    // ...
}
```
Now THAT was easy, huh? 

---
## Complete Initial Integration

1. Finish integrating the `PostsRepository` into the `PostsController` by calling it within the create, update, and delete methods.


2. Follow the same pattern to integrate `User` repository/controller. Ignore `getByUsername` and `getByEmail` for now.


3. If you need more acute querying for your endpoints, see [Data Persistence, Pt II](14-data-persistence-iii.md).

---
## The Moment of Truth.

Now, it's time to spin up your application! 

1. Start it, then check your database to see if the `posts` and `users` tables were created!

2. Manually insert a few post and user records.

3. Use Swagger or Postman to fetch all posts and fetch one post.

4. Use Swagger or Postman to fetch all users and fetch one user
        
Congratulations! You have connected your `Post` and `User` Java classes to your database using JPA. But... we're not done yet! On to part 2 of data persistence!



---
### Further Reading
- [What is Dependency Injection?](http://stackoverflow.com/questions/130794/what-is-dependency-injection)
- [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection)
- [Spring Beans and dependency injection](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-spring-beans-and-dependency-injection.html)

## Next Up: [Data Persistence, Part 2](13-data-persistence-ii.md)
