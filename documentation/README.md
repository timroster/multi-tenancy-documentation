# Multi-tenancy Assets for IBM Partners to build SaaS (Documentation)

### Documentation Overview

* [Introduction](./README_introduction.md)
* Development of Microservices
    * [Quarkus Backend Service Code](./development_of_microservices/backend-service-impl.md)
    * [Quarkus Backend Service Container](./development_of_microservices/backend-service-container.md)
    * [Vue.js Frontend Service Code](./development_of_microservices/frontend-service-code.md)
    * [Vue.js Frontend Service Container](./development_of_microservices/frontend-service-container.md)
    * [Externalization of Variables in Backend Microservices](./development_of_microservices/externalization-of-variables-in-backend-microservices.md)
    * [Externalization of Variables in Frontend Microservices](./development_of_microservices/externalization-of-variables-in-frontend-microservices.md)
    * [Local Development of Services](./development_of_microservices/local-development.md)
    * [Authentication Flow (AppID, backend, frontend)](./development_of_microservices/authentication-flow-appip-backend-frontend.md)
* Creation of managed IBM Cloud Services
    * Database
        * [Programmatic Creation of Postgres](./creation-of-managed-ibm-cloud-services/create-postgres.md)
        * [Programmatic Configuration of Postgres including Schema](./creation-of-managed-ibm-cloud-services/create-postgres-schema.md)
    * Authentication
        * [Programmatic Creation of AppID](./creation-of-managed-ibm-cloud-services/create-appid.md)
        * [Programmatic Configuration of AppID](./creation-of-managed-ibm-cloud-services/configure-appid.md)
* Serverless via IBM Code Engine
    * Architecture
    * Initial Setup via Scripts
        1. [Create the instances](./serverless-via-ibm-code-engine/ce-setup-create-the-instances.md)
        2. [Verify the created instances](./serverless-via-ibm-code-engine/ce-verify-the-created-instances.md) 
    * [CI/CD](./serverless-via-ibm-code-engine/serverless-cicd.md)
    * [Onboarding](./serverless-via-ibm-code-engine/code-engine-onboarding.md)
    * [Observability (logging, monitoring, vulnerabilities)](documentation/observability.md)
    * [Billing](./serverless-via-ibm-code-engine/code-engine-billing.md)
    * [Clean Up](./serverless-via-ibm-code-engine/ce_clean_up.md)
* Kubernetes via IBM Kubernetes Service and IBM OpenShift
    * Architecture
    * [Initial Setup via Scripts](./automation/terraform/3-Provisionning-A-Kubernetes-Based-Infrastructure.md)
    * CI/CD DevSecOps
        * [Overview](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/devsecops-overview.md)
        * CI
            * [CI pull request](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/ci-pull-request.md)
            * [CI pipeline](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/ci-pipeline.md)
        * CD
            * [CD pull request](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/cd-pull-request.md)
            * [CD pipeline](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/cd-pipeline.md)
        * [Security and Compliance](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/security-and-compliance.md)
        * Setup of the Toolchains
            * [CI Toolchains](./k8s/3-ci-cd/README_ci.md)
            * [CD Toolchains](./k8s/3-ci-cd/README_cd.md)
    * [Onboarding](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/k8s-onboarding.md)
    * [Observability (logging, monitoring, vulnerabilities)](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/observability.md)
    * [Billing](./kubernetes-via-ibm-kubernetes-service-and-ibm-openshift/k8s-billing.md)
