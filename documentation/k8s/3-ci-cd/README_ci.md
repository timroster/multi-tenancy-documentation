## Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CI) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kuberenets cluster.

# Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the multitenancy-backend and multitenancy-frontend repos to your GitHub account
- An instance of the IBM Cloud Secrets Manager service
- An IBM Cloud Kubernetes Service cluster or An IBM Cloud OpenShift cluster
- Create a namespace in the IBM Cloud Container Registry (access via hamburger menu->Container Registry)

# Create the toolchains

login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region:

![DevSecOps](/images/k8s/1-devOpsSelectRegion.png)

Clieck `Create Toolchain` and select the `DevSecOps` filter:

![DevSecOpsFiltered](/images/k8s/2-filterToDevSecOpsToolchains.png)


# Create CI toolchain for backend

Let's start with CI for backend.  Click the CI tile to launch the setup wizard:

![CI Setup Wizard](/images/k8s/3-cISetupWizard.png)

Name the toolchain and click `Continue`

![Name CI Toolchain](/images/k8s/4-nameCiToolchain.png)

Select the backend repository which you cloned to your GitHub account.  Do not use the IBM repo as pictured below, as you will be unable to make changes later.  Click `Continue`.

![Backend repo](/images/k8s/5-bringYourOwnAppCiBackend.png)

The CI pipeline stores some of its output to an Inventory repository, which will be stored in your IBM Cloud GitLab account (all IBM Cloud users are provided access to GitLab).

Name the Inventory repository and click `Continue`:

![Inventory repo](/images/k8s/6-createInventoryRepocIBackend.png)

Similarly, name the Issues repo and click `Continue`:

![Issues rep](/images/k8s/7-createIssuesRepocIBackend.png)

Accept the default recommended Secrets Manager and click `Continue`

![Select Secrets Manager](/images/k8s/8-createSecretsManager.png)

Configure an existing Secrets Manager service by typing its instance name and resource group, then click `Continue`

![Configure Secrets Manager](/images/k8s/9-configureSecretsManager.png)

Name the Evidence storage repostitory.  The COS bucket can be added later if required, so leave this unchecked for now.  Click `Continue`:

![Configure Secrets Manager](/images/k8s/10-configureEvidenceStorage.png)

Accept the default Tekton pipeline definitions from the IBM repository.  This ensures your toolchain stays up to date with the latest additions.

![Use existing Tekton pipelines](/images/k8s/11-useExistingTektonPipelines.png)

Create a new API key (or use an existing one) for the toolchain to access your IBM Cloud account.  Specify the details of an existing IBM Cloud Container Registry (you may need to type this manually).  Specify the details of an existing IBM Cloud Kubernetes Service (IKS) cluster.  Click `Continue`:

![Registry and cluster config](/images/k8s/12-deploymentTarget.png)

Create a new Image Signing Key by speifying a name and email.  Click `Continue`:

![Signing key config1](/images/k8s/13-signingKey1.png)
![Signing key config2](/images/k8s/14-signingKey2.png)

DevOps Insights is automatically part of the toolchain being created, so select this instance and click `Continue`:

![DevOpsInsights config](/images/k8s/15-devOpsInsights.png)

SonarQube is automatically part of the toolchain being created, so select this instance and click `Continue`:

![SonarQube config](/images/k8s/16-sonarQube.png)

Select optional tools and click `Continue`:

![Optional tools](/images/k8s/17-optionalTools.png)

Review the summary and click `Create`:

![Summary](/images/k8s/18-summary.png)

After a few moments, the toolchain is displayed:

![Toolchain created](/images/k8s/18-toolchainCreated.png)

Click the CI pipeline, then `Environment properties`.  

![Environment Props](/images/k8s/19-environmentProps.png)

Add or update the following properties:

| Key  | Value | Type |
| ------------- | ------------- | ------------- |
| branch  | `main`  | Text |
| microservice-name  | `backend` | Text |
| multi-tenancy  | your repo, e.g. `https://github.com/<your-org>/multi-tenancy`  | Text |
| multi-tenancy-backend  | your repo, e.g. `https://github.com/<your-org>/multi-tenancy-backend`  | Text |
| multi-tenancy-frontend  | your repo, e.g. `https://github.com/<your-org>/multi-tenancy-frontend`  | Text |
| opt-in-sonar  | delete `true` (set to blank)  | Text |
| pipeline-config-branch | update from `.pipeline-config.yaml` to `main`  | Text |
| repository | select your repo, e.g. `https://github.com/<your-org>/multi-tenancy-backend` from the list.  Also set JSON filter to `parameters.repo_url`  | Tool Integration |

# Create CI toolchain for frontend

Repeat the above steps to create a second toolchain for frontend, changing only the following values:

Select the frontend repository which you cloned to your GitHub account.  Do not use the IBM repo as pictured below, as you will be unable to make changes later.

![Frontend repo](/images/k8s/21-bringYourOwnAppCiFrontend.png)

The environmental properties for the frontend CI pipeline should be set as follows:

Add or update the following properties:

| Key  | Value | Type |
| ------------- | ------------- | ------------- |
| branch  | `main`  | Text |
| microservice-name  | `frontend` | Text |
| multi-tenancy  | your repo, e.g. `https://github.com/<your-org>/multi-tenancy`  | Text |
| multi-tenancy-backend  | your repo, e.g. `https://github.com/<your-org>/multi-tenancy-backend`  | Text |
| multi-tenancy-frontend  | your repo, e.g. `https://github.com/<your-org>/multi-tenancy-frontend`  | Text |
| opt-in-sonar  | delete `true` (set to blank)  | Text |
| pipeline-config-branch | update from `.pipeline-config.yaml` to `main`  | Text |
| repository | select your repo, e.g. `https://github.com/<your-org>/multi-tenancy-frontend` from the list.  Also set JSON filter to `parameters.repo_url`  | Tool Integration |

# Testing the CI pipelines

Test the CI pipelines by triggering a pipeline run, using dev mode:

![Pipeline run trigger](/images/k8s/19-environmentProps.png)