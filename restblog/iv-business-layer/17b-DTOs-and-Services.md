# DTOs and Services

We have wrapped our heads around Rest Controllers, Data Persistence, and Entities.

Our API can accept requests, retrieve/persist data, and send responses! That's awesome!

However, we are faced with a few issues:

- Serialization: all of these `@JsonIgnoreProperties` gets old and repetitive
- Not all the data we store is the data we need to send
- The shape of our database is great for us, not so much for the client-side (*lots* of nested objects)

Getting us closer to being fully RESTful means we need to put a middle layer of objects 
between the data access layer and everything else.

## Data Transfer Objects (DTOs)

For our purposes, DTOs are simply POJOs which represent the state of data from the perspective
of *how* we want to represent that state to the outside world.

Let's say that for an existing `User` record, we only want to allow the 
client to update the `email` and `username` fields (we'll talk about passwords separately).

We don't really want the client developer to have to parse through something like this:

```JSON
{
  "id": 0,
  "username": "string",
  "email": "string",
  "password": "string",
  "createdAt": "2022-05-24T13:00:18.732Z",
  "role": "USER",
  "posts": [
    {
    "id": 1,
    "title": "string",
    "content": "string"
    },
    {
      "id": 2,
      "title": "string",
      "content": "string"
    }
  ]
}
```

Just to update this:

```JSON
{
  "id": 1,
  "username": "string",
  "email": "string"
}
```

As well, we would rather not place `@JsonIgnoreProperties` all over our entity fields.
That can become unreadable and unmaintainable very quickly.

---

### 🚨 `UpdateUserDto`

1. Inside the `web` package, create a new package named `dto`

2. In `dto`, create a class named `UpdateUserDto` and give it the following fields:

```JAVA
public class UpdateUserDto{
    private long id;
    private String username;
    private String email;
    
    //TODO: add constructors, getters, and setters
    
    //TODO: add a toString() override
}
```

3. Refactor your `UsersController` to include the following method:

```JAVA
@PutMapping
public void update(@RequestBody UpdateUserDto updateUserDto){
    System.out.println(updateUserDto);
}
```

4. Test this (`PUT /api/users`) through Swagger or Postman. We are simply looking for console output at this point.

5. Now, add the following public method to your `UserService`:

```JAVA
public void updateUser(UpdateUserDto updateUserDto){
    User user = usersRepository.findById(updateUserDto.getId()).orElseThrow();
    
    if(updateUserDto.getUsername() != null && !updateUserDto.getUsername.isEmpty()){
        user.setUsername(updateUserDto.getUsername());
    }
    if(updateUserDto.getEmail() != null && !updateUserDto.getEmail.isEmpty()){
        user.setEmail(updateUserDto.getEmail());
    }
    usersRepository.save(user);
}
```

6. Again, test this update with Swagger or Postman! Check the database to ensure the change persisted with no additional side effects.

---

### 🚨`CreateUserDto`

Same goes for this scenario!

The client is really only concerned with setting the new user's `username`, `email`, and `password`.

1. Make a DTO which reflects that idea:

```JAVA
public class CreateUserDto{

    private String username;
    private String email;
    private String password;
    
    //TODO: constructors, getters, and setters
    
    //TODO: toString() override
}
```

2. Now, update `UsersController.create()`:

```JAVA
@PostMapping
public void create(@RequestBody CreateUserDto createUserDto){
    System.out.println(createUserDto);
//        userService.getUsersList().add(newUser);
}
```

3. Test via Swagger or Postman and check your console!

4. Now, update the UserService to add the following method (no copypasta!):

```JAVA
public void createUser(CreateUserDto createUserDto){
    usersRepository.save(new User(
            createUserDto.getUsername(), 
            createUserDto.getEmail(), 
            createUserDto.getPassword()));
}
```

5. Test via Swagger and Postman, then check your database to ensure the data persisted!

6. Test the client-side `/register` view to see if changes on the frontend are needed!

---

### 🚨`ReadUserDto`

1. Follow the above pattern, creating a `ReadUserDto` and placing within it the fields you need for `GET` requests on the `UsersController`.

2. Do this *one* controller method at a time:
   1. refactor the `UserController` `@GetMapping` methods which currently use the `User` entity to instead use the `ReadUserDto`.

   2. Once tested via Swagger or Postman, refactor the UserService read methods to return the `ReadUserDto` (or `List<ReadUserDto>`).

   3. Do any refactoring needed on the frontend.

   4. Move on to the next `@GetMapping` method

3. Above all, test each endpoint and the frontend carefully to ensure we didn't make new bugs!

---

## Post DTOs

Following the same patterns above, begin refactoring your Post-related code to use DTOs!

Decide:

- What information is necessary for the client?
- What *should* be allowed to be created or updated on the frontend?

Again, 🧪 TEST, 🧪 TEST, 🧪 TEST!


## Next Up: [Service Layer Abstraction/Encapsulation](17c-decoupled-services.md)
