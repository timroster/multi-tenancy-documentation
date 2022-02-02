# Architecture

The easiest way to get started is to set up the sample application for two tenants on the IBM Cloud using serverless technology. The following diagram describes the serverless architecture of the simple e-commerce application which has two images (backend and frontend).

Isolated Compute:

* One frontend container per tenant
* One backend container per tenant
* One App ID instance per tenant
* One Postgres instance (with one database) per tenant

Shared CI/CD:

* One code base for frontend and backend services
* One image for frontend service
* One image for backend service
* One toolchain for all tenants (with four pipelines)

<kbd><img src="../images/introduction/multi-tenant-app-architecture.png" /></kbd>

Used IBM Services:

* IBM Code Engine
* IBM Container Registry
* IBM App ID
* IBM Postgres
* IBM Toolchain

Used Technologies:

* Quarkus
* Vue.js and nginx
* Bash scripts