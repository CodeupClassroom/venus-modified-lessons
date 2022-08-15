# REST Blog

This project is a sandbox for a full-stack Single Page Application solution for a basic blogging app. The backend is Java+Spring and the frontend uses a custom SPA framework built by Codeup staff.

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

## Next Up: [A Spring Overview](./i-intro/1-overview.md)
