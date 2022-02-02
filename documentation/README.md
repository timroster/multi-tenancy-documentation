# Multi-tenancy Assets for IBM Partners to build SaaS (Documentation)

### Documentation Overview

* Introduction
* Development of Microservices
    * [Quarkus Backend Service Code](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/backend-service-impl.md)
    * [Quarkus Backend Service Container](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/backend-service-container.md)
    * [Vue.js Frontend Service Code](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/frontend-service-code.md)
    * [Vue.js Frontend Service Container](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/frontend-service-container.md)
    * [Externalization of Variables in Backend Microservices](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/externalization-of-variables-in-backend-microservices.md)
    * [Externalization of Variables in Frontend Microservices](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/externalization-of-variables-in-frontend-microservices.md)
    * [Local Development of Services](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/local-development.md)
    * [Authentication Flow (AppID, backend, frontend)](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/development_of_microservices/authentication-flow-appip-backend-frontend.md)
* Creation of managed IBM Cloud Services
    * Database
        * [Programmatic Creation of Postgres](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/creation-of-managed-ibm-cloud-services/create-postgres.md)
        * [Programmatic Configuration of Postgres including Schema](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/creation-of-managed-ibm-cloud-services/create-postgres-schema.md)
    * Authentication
        * [Programmatic Creation of AppID](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/creation-of-managed-ibm-cloud-services/create-appid.md)
        * [Programmatic Configuration of AppID](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/creation-of-managed-ibm-cloud-services/configure-appid.md)
* Serverless via IBM Code Engine
    * Architecture
    * Initial Setup via Scripts
        1. [Create the instances](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/serverless-via-ibm-code-engine/ce-setup-create-the-instances.md)
        2. [Verify the created instances](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/serverless-via-ibm-code-engine/ce-verify-the-created-instances.md) 
    * [CI/CD](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/serverless-via-ibm-code-engine/serverless-cicd.md)
    * [Onboarding](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/serverless-via-ibm-code-engine/code-engine-onboarding.md)
    * [Observability (logging, monitoring, vulnerabilities)](documentation/observability.md)
    * [Billing](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/serverless-via-ibm-code-engine/code-engine-billing.md)
    * [Clean Up](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/serverless-via-ibm-code-engine/ce_clean_up.md)
* Kubernetes via IBM Kubernetes Service and IBM OpenShift
    * Architecture
    * [Initial Setup via Scripts](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/automation/terraform/3-Provisionning-A-Kubernetes-Based-Infrastructure.md)
    * CI/CD DevSecOps
        * [Overview](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/devsecops-overview.md)
        * CI
            * [CI pull request](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/ci-pull-request.md)
            * [CI pipeline](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/ci-pipeline.md)
        * CD
            * [CD pull request](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/cd-pull-request.md)
            * [CD pipeline](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/cd-pipeline.md)
        * [Security and Compliance](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/security-and-compliance.md)
        * Setup of the Toolchains
            * [CI Toolchains](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/k8s/3-ci-cd/README_ci.md)
            * [CD Toolchains](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/k8s/3-ci-cd/README_cd.md)
    * [Onboarding](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/k8s-onboarding.md)
    * [Observability (logging, monitoring, vulnerabilities)](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/observability.md)
    * [Billing](https://github.com/IBM/multi-tenancy-documentation/blob/main/documentation/kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/k8s-billing.md)
