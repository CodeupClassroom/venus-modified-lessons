# Alternate delivery of the Security sub-module
1. Read lessons: 
  - https://github.com/CodeupClassroom/venus-modified-lessons/blob/main/restblog/v-security/18-intro-to-security.md
  - https://github.com/CodeupClassroom/venus-modified-lessons/blob/main/restblog/v-security/19-implementing-oauth.md

3. make the bulk changes for security as specified in:
	https://github.com/CodeupClassroom/venus-modified-lessons/blob/main/restblog/v-security/19-implementing-oauth.md

2. prep the db for security:
	
	a. empty or drop post_category
	
	b. empty or drop posts
	
	c. drop users

	NOTE: truncate does not work with the fk constraints :(

3. 	Test and play around by:

	a. registering a new user

	b. logging in as the new user

	c. viewing the browser's Local Storage and see the tokens

	d. checking out the access token in jwt.io

	e. manually deleting the tokens in the browser
	
		i. try to go to the User information screen

		ii. what do you expect to see?

4. 	Encrypt the passwords when adding a new user or changing a user's password
	
	a. in your UsersController, add a PasswordEncoder field

	b. make sure the PasswordEncoder is injected by Spring

	c. in create user, set the default role to USER

	d. in create user and and change password, encrypt the password by calling `passwordEncoder.encode(plainTextPassword);`

	and save the encoded password to the db

5. change your /me endpoint to return the currently logged in user

	a. add an OAuth2Authentication parameter to your endpoint's method. See this lesson for information about this parameter:
https://github.com/CodeupClassroom/venus-modified-lessons/blob/main/restblog/v-security/21-using-auth-as-resource.md

	b. use your repository to fetch the user associated with the logged in user's "name"

	c. return that user

	d. test by logging in as different users and viewing the user info


6. removing stale access tokens

	a. login to the system

	b. restart the backend

	c. view User info
		what is happening?

	d. enable stale token removal by adding these functions to auth.js

```js
export function isLoggedIn() {
    if(localStorage.getItem('access_token')) {
        return true;
    } else {
        return false;
    }

}

//  returns an object with user_name and authority from the access_token
export function getUser() {
    const accessToken = localStorage.getItem("access_token");
    if(!accessToken) {
        return false;
    }
    const parts = accessToken.split('.');
    const payload = parts[1];
    const decodedPayload = atob(payload);
    const payloadObject = JSON.parse(decodedPayload);
    const user = {
        userName: payloadObject.user_name,
        role: payloadObject.authorities[0]
    }
    return user;
}

export function getUserRole() {
    const accessToken = localStorage.getItem("access_token");
    if(!accessToken) {
        return false;
    }
    const parts = accessToken.split('.');
    const payload = parts[1];
    const decodedPayload = atob(payload);
    const payloadObject = JSON.parse(decodedPayload);
    return payloadObject.authorities[0];
}

export async function removeStaleTokens() {
    console.log("Removing stale tokens...");

    // clear tokens from localStorage if backend tells us the tokens are invalid
    // make the request
    const request = {
        method: 'GET',
        headers: getHeaders()
    };
    await fetch(`/api/users/me`, request)
        .then((response) => {
            // if fetch error then you might be using stale tokens
            if (response.status === 401) {
                window.localStorage.clear();
            }
        }).catch(error => {
            console.log("FETCH ERROR: " + error);
        });
}
```

	e. change top of createView to this:

```js
import {getHeaders, removeStaleTokens} from "./auth.js";

...

export default async function createView(URI) {
    // createView must wait for stale token removal before finishing view creation
    await removeStaleTokens();
```

	f. test!


7. add the Authenticated User as the author of an added post

	a. in the frontend, when creating the POST and DELETE requests, create the headers by calling getHeaders() from auth.js

	b. test by creating another login 
		be sure to change USER to ADMIN for one of the logins in the database

8. try to add a post as someone who is not logged in

	a. in PostsController, add @PreAuthorize annotations to ADD DELETE and EDIT methods. See this lesson for more info:
  https://github.com/CodeupClassroom/venus-modified-lessons/blob/main/restblog/v-security/22-method-specific-access.md

		i.e., you MUST be a USER OR ADMIN for those functions to be called

	b. test!

9. add title and content validation for PostsController
	 a. in the ADD and UPDATE methods, if the title is null OR the length is < 1 then throw a 400 exception

	 b. do the same for content

10. make sure that only the author of the post OR an admin can delete or edit someone else's post

    a. add `OAuth2Authentication auth` as a parameter

    b. compare the author of the post to the user who is currently logged in.
    	if the currently logged in user IS NOT an admin AND IS NOT the author of the post then throw a 401 exception




----------------------------------

NICE THINGS FOR US TO ADD:

	a. On the frontend, provide Logout functionality
	
	b. On the frontend, show who is logged in on right side of navbar or Not logged in if anonymous
	
	c. On the frontend:
		1. hide login if logged in 
		2. hide logout if anonymous
		3. hide the User Info screen if anonymous

	d. On the frontend, hide post edit and delete buttons if not your posts AND you are not an admin

	e. On the frontend, have user info screen print appropriate message if you are not logged in
		Test it by typing the user info screen URL in the browser's address line
			e.g., http://localhost:8081/me

	f. adding categories to a post
