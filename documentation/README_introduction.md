# Introduction

... TBD
### Platform Options

The following diagram shows the different platform options. This repo contains the IBM Cloud platforms so far (blue rectangle). 

More options are planned to be added. For example with Satellite the SaaS application can be deployed on-premises to client data centers, but managed centrally. Additionally the same SaaS application can be deployed on other managed OpenShift services like AWS ROSA and Azure ARO.

**Serverless on IBM Cloud**

The easiest way to get started is to use serverless. The repo describes how to use IBM Code Engine to run the application logic, IBM App ID for authentication, IBM Postgres for persistence and IBM Toolchain for CI/CD. Scripts are provided to make the setup as easy as possible.

**Managed Kubernetes and OpenShift on IBM Cloud**

For more advanced cloud-native applications Kubernetes and OpenShift can be used. Compute isolation can be done either by sharing clusters and using Kubernetes namespaces/OpenShift projects or by having separate clusters for tenants. For authentication the managed services App ID and Postgres can be used, but they can also be replaced by other managed services or services running within the clusters.

For CI/CD the IBM DevSecOps reference architecture based on IBM Toolchain is used which is also the internal IBM standard and which guarantees compliance for regulated industries.

![](./images/introduction/SaaS-Options.png)

### Sample Application

The project comes with a simple e-commerce example application. A SaaS provider might have one client selling books, another one selling shoes.

![](./images/introduction/example-app.png)

### Repositories

This repo is the 'parent repo' including documentation and global configuration. The other three repos contain the implementation of the microservices and the serverless pipelines.

* [multi-tenancy](https://github.com/IBM/multi-tenancy) - this repo (parent repo)
    * Documentation
    * Global configuration
    * CD pipeline
    * Scripts

* [multi-tenancy-backend](https://github.com/IBM/multi-tenancy-backend) - backend microservice
    * Code
    * CI pipeline

* [multi-tenancy-frontend](https://github.com/IBM/multi-tenancy-frontend) - frontend microservice   
    * Code
    * CI pipeline

* [multi-tenancy-serverless-ci-cd](https://github.com/IBM/multi-tenancy-serverless-ci-cd) - CI and CD pipelines for serverless