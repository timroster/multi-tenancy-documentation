# Externalization of Variables in Backend Microservices

We need to configure for our backend microservice following information in externalized variables.

* We need to securely connect to the Postgres database

    - Postgres certificate
    - Postgres connection url
    - Postgres user and password

* We need to configure the OpenID connect information for Quarkus 


> For the basics about the externalization of variables, you can also visit that blog post called [`HOW TO USE ENVIRONMENT VARIABLES TO MAKE A CONTAINERIZED QUARKUS APPLICATION MORE FLEXIBLE`](https://suedbroecker.net/2021/05/31/how-to-use-environment-variables-to-make-a-containerized-quarkus-application-more-flexible/)