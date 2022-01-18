## Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CD) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kuberenets cluster.

# Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the multitenancy-backend and multitenancy-frontend repos to your GitHub account
- An instance of the IBM Cloud Secrets Manager service
- An IBM Cloud Kubernetes Service cluster or An IBM Cloud OpenShift cluster
- Create a namespace in the IBM Cloud Container Registry (access via hamburger menu->Container Registry)

# Create the toolchains

login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region:

![DevSecOps](./images/1-devOpsSelectRegion.png)

Clieck `Create Toolchain` and select the `DevSecOps` filter:

![DevSecOpsFiltered](./images/2-filterToDevSecOpsToolchains.png)

# Create CD toolchain

Click the CD tile to launch the setup wizard:

![CD Setup Wizard](./images/3-cISetupWizard.png)

Name the toolchain and click `Continue`:

![Name CD Toolchain](./images/22-cdPipeline.png)

Select the Inventory repository, based on the repository used in the CI pipeline and click `Continue`:

![Inventory repo](./images/23-createInventoryRepocDBackend.png)

Select the Issues repository, based on the repository used in the CI pipeline and click `Continue`:

![Issues repo](./images/24-createIssuesRepocDBackend.png)

Select the repository for Pipeline configuration, Use the multi-tenancy repository which you cloned to your GitHub account.  Do not use the IBM repository as pictured below, as you will be unable to make changes later.  Click `Continue`.

![Pipeline config repo](./images/25-pipelineConfigCd.png)

Click `Continue` to use the Secrets Manager service:

![Pipeline config repo](./images/26-secretsManagerCd.png)

Specify the connection details for your secrets manager:

![Secrets manager](./images/27-secretsManagerConfigCd.png)

Use the default Evidence Storage repository, based on the repository used in the CI pipeline and click `Continue`:

![Evidence Storage](./images/28-evidenceStorageCd.png)

WORK IN PROGRESS
