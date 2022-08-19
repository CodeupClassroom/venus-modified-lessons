# RESTful APIs using Spring (Boot)

This module combines learning how to use Spring to build a RESTful API as a backend solution for a fullstack application with a running fullstack project of a blogging application, the REST Blog. The REST Blog project is a sandbox for a full-stack Single Page Application solution for a basic blogging app. The backend is Java+Spring and the frontend uses the Jalopy framework, covered in the SPA module. 

NOTE: future curriculum revisions will use Vue as the frontend framework.

## General Architecture

The application is divided into 2 pieces:

- A backend Spring Server which handles:
    - Security
  - Data Persistence (communication w/ our DB)
  - Data Resources (as JSON)
  - Serving Static Resources (the frontend)
    
- A frontend JavaScript Client which is responsible for:
    - Rendering the UI
    - Handling interactivity with the UI
    - Making requests for data resources to the backend
    
In addition, this application comes with a dependency on ```springdoc-openapi-ui```

We can use this tool to quickly test resource endpoints by navigating to http://localhost:8080/swagger-ui.html

Swagger is an excellent tool which provides full documentation of exposed endpoints, including sample requests and responses - makes the job of testing very smooth! 

Or... you can use **Postman** for documentation and testing :)

This module is divided into the following sub-modules:

1. [Introduction](./i-intro/1-overview.md)
2. [REST basics](./ii-rest-and-relationships/5-rest.md)
3. [The data access layer](./iii-data-persistence/12-data-persistence.md)
4. [The business layer](./iv-business-layer/1-overview.md)
5. [The security layer](./v-security/1-overview.md)

## Next Up: [Introduction to Spring](./i-intro/1-overview.md)
