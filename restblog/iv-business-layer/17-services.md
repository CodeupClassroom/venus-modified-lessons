# Services

A service is a special type of class that provides a way to interact with some
functionality in the application in a simpler way. A service can provide all
sorts of different functionality, for example:

- Send emails
- Uploading files
- Interacting with database tables (JPA repositories)
- Interacting with an external API

Services allow us to decouple the specific logic for performing actions, like the
above, from the rest of our application and call those methods from anywhere.


---
## US13: As a user, I can receive an email when new posts are created.
In order to use the default email implementation with Spring you will need to add the following dependency to your `pom.xml` file:\

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
    <version>2.1.2.RELEASE</version>
</dependency>
```

Here is an example service class which could be used to notify the user when a new Post has been created. Create a new package called `services` and put the `EmailService` class in that package.

```java
import com.example.restblog.data.Post;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.mail.MailException;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;

@Service("mailService")
public class EmailService {

    @Autowired
    public JavaMailSender emailSender;

    @Value("${spring.mail.from}")
    private String from;

    public void prepareAndSend(Post post, String subject, String body) {
        SimpleMailMessage msg = new SimpleMailMessage();
        msg.setFrom(from);
        msg.setTo(post.getAuthor().getEmail());
        msg.setSubject(subject);
        msg.setText(body);

        try{
            this.emailSender.send(msg);
        }
        catch (MailException ex) {
            // simply log it and go on...
            System.err.println(ex.getMessage());
        }
    }
}
```

The `@Service` annotation tells Spring that this is a class that will be managed by spring.

The JavaMailSender class implementation will require the following properties defined in your `application.properties` file:

```text
spring.mail.host=smtp.mailtrap.io
spring.mail.port=25
spring.mail.username=username
spring.mail.password=password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.from=admin@example.com 
```
These credentials will be used by the email service of your choice to send the emails. We will use [mailtrap.io](https://mailtrap.io) for testing purposes. You'll have to sign up for an account and get your own username and password.

Once you have your service ready to go, you can use **dependency injection** to include the service in any class, in this case to the `PostsController`.


```java
@RestController
class PostsController {
    // ...
    private final EmailService emailService;

    public PostsController(EmailService emailService) {
        this.emailService = emailService;
    }
    // ...
}
```


---
### Now, we have three distinct layers of our server-side application (aside from security):

- Data Transfer Layer (Controllers)
  

- NEW: Business Layer (Services)
  

- Data Access Layer (Repositories)

## Next Up: [DTOs and Services](17b-DTOs-and-Services.md)
