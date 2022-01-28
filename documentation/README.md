# Multi-tenancy Assets for IBM Partners to build SaaS (Documentation)

### Introduction

A key benefit of the cloud is the ability to deploy software for multiple consumers without having to install it redundantly on-premises. When software is provided as a managed service (SaaS), costs can be reduced for the deployments and the operations of applications. Additionally SaaS can be scaled and new consumers can be added easily.

In order to leverage these advantages, applications need to be designed, so that they can support multiple tenants. Often tenants are not single users, but clients of SaaS providers with their own corporate authentication mechanisms. When running SaaS for multiple tenants, it's often required to keep the workloads isolated from each other for security reasons. For example, typically separate databases are used for tenants.

At the same time common deployment and operation models are required, so that new SaaS versions can be deployed to different tenants in an unique and efficient way.

This project aims to support IBM partners to build SaaS for different platforms including Kubernetes, OpenShift, Serverless and Satellite. The used sample application, which contains two containers, is the same one for all platforms; the CI/CD mechanisms slightly differentiate between the platforms.
