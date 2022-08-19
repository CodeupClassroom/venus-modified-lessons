# The User

## What is the `User`?

In many web applications, there are often two main ideas:

1. What basic function does our application perform?
2. Who interacts with the application and what can they ask it to do?

By creating a `Post` object and a controller to help perform CRUD, we nailed down #1: our application performs the functionality of creating, reading, updating, and deleting blog posts.

Now, we need to fill out #2: **Who interacts with the application and what can they ask it to do?**

---

## How to define the `User`

We need to think about ***necessary*** information for the user of our application.

**Internally**:

- How do we identify them?
- When were they created?
- What are they allowed to do?

**Client-facing**:

- What is their name (username, real name, email, etc)?
- Can they customize their presence (avatar, photo, bio, etc)?

**General**:

- Over which content (Posts) do they have ownership?
- How does a user validate access to our application (passwords)?

For now, let's focus on the basics of our `User` by creating a model with a few key properties:

---

### The following is a user story list to be implemented in your blog application:

## US5: As a user, I can register with the application

---

### US5-A: Implement the `User` class

#### 1. Inside the `data` package, create a class named `User`


#### 2. Give the `User` the following private fields:

- long id
- String username
- String email
- String password
- LocalDate createdAt
- Role role

For our `Role`, we will create an `enum` inside the `User` class like so:

```JAVA
public class User {

    // ...Your fields above here

    public enum Role {USER, ADMIN}

    ;
```

!!! 🧠 `enum`

In programming, enums (or, enumerated values) represent constant values which share a common purpose.

For example, for a Role enum, you may have DEVELOPER, QA, and MANAGER roles.

In code, this would look like:

  ```JAVA
    enum Role {DEVELOPER, QA, MANAGER}
  ```

And could be invoked externally as:

  ```JAVA
    private Role role = Role.DEVELOPER;
  ```

#### 3. Create our standard POJO items:

- Empty and full constructors
- All getters and setters

---

### US5-B: Implement the `UsersController`

We can now set up a REST Controller for the purpose of running CRUD operations related to the `User`.

***After completing each method, be sure to test in Swagger or Postman***

#### 1. In the package `web`, create a class called `UsersController`.


#### 2. While filling out the class, follow the same pattern as found in [REST Controllers](6-rest-controllers.md) and [REST Controllers, Part 2](7-rest-controllers.md).
   
- You will need methods for `getAll`, `getById`, `create`, `update`, and `deleteById` with their respective annotations.


- **Make sure the class' `@RequestMapping` value is set to `/api/users`**. IMPORTANT: register user needs a special endpoint `/api/users/create`
    

- ***WARNING, be very careful if you copypasta***. It will be EASY for you to forget to replace one of those `Post` references with `User` and waste your own time tracking down the issue.


#### 3. Test test test (with Swagger or Postman)

---

### US5-C: Register the `User` Client-Side

`resources/static/js/views/Login.js` shows us how we can begin a view for creating users.

#### 1. In `resources/static/js/views/`, create `Register.js`


#### 2. Following what you see in `Login.js`, create a form allowing a new user to register with our application.
    - They need to be able to enter their `username`, `email`, and `password`.
    - A button will be needed for allowing the user to submit their inputs.


#### 3. **Ensure the password input is hidden.** (there is an HTML input attribute for this!)


#### 4. Open `js/router.js`. In here, you will find a template for how to add a property to allow for the registration view to be rendered.

```JAVASCRIPT
'/login': {
   returnView: Login,
   state: {},
   uri: '/login',
   title: "Login",
   viewEvent: LoginEvent
}
```

- Make an object with the same properties and replace the `login` references with `register`.


#### 5. Notice `viewEvent: LoginEvent` above? You will need your own function to serve as a callback for `/register`.
 - When the user submits the form, your JavaScript listens for that button click.
 - You will need to add an event listener to that button when it is rendered.
 - After the click, grab the input values from the DOM, bundle them together in an object which mimics the
   backend `User` properties.
 - Do not worry with the `Role`, `Posts`, or `Id`. This is only for registration!

Still in your `RegisterEvent` function, add this bit to the bottom:

```JAVASCRIPT
let request = {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: **YOUR USER OBJECT**
};

fetch("http://localhost:8080/api/users", request)
    .then((response) => {
        console.log(response.status)
        createView("/");
    });
```

Now, add `RegisterEvent()` as the `viewEvent` property for `/register` in `route.js`:

```JAVASCRIPT
const routes = {
    // ...additional routes
   
    '/register': {
        returnView: Register,
        state: {},
        uri: '/register',
        title: "Register",
        viewEvent : RegisterEvent
    }
    
    // additional routes...
}
```

Once you plug this in, you now need to add `/register` to the list of paths inside your `ViewController`. From that point you are ready to begin testing the creation of a `User`!

Check the `UsersController` to see if your new User is printed out on your `POST` method.

---


*Looking forward, we need more than the ability to perform basic CRUD for our `User`.*

## Next Up: [The User, Part 2](8a-the-user-pt-2.md)

