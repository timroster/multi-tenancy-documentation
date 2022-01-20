# Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CI) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kuberenets cluster.

## Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the multitenancy-backend and multitenancy-frontend repos to your GitHub account
- An instance of the IBM Cloud Secrets Manager service
- An IBM Cloud Kubernetes Service cluster or An IBM Cloud OpenShift cluster
- Create a namespace in the IBM Cloud Container Registry (access via hamburger menu->Container Registry)

## Create the toolchains

login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region:

![DevSecOps](/documentation/images/cicd-k8s/1-devOpsSelectRegion.png)

Clieck `Create Toolchain` and select the `DevSecOps` filter:

![DevSecOpsFiltered](/documentation/images/cicd-k8s/2-filterToDevSecOpsToolchains.png)


## Create CI toolchain for backend

Let's start with CI for backend.  Click the CI tile to launch the setup wizard:

![CI Setup Wizard](/documentation/images/cicd-k8s/3-cISetupWizard.png)

Name the toolchain and click `Continue`

![Name CI Toolchain](/documentation/images/cicd-k8s/4-nameCiToolchain.png)

Select the backend repository which you cloned to your GitHub account.  Do not use the IBM repo as pictured below, as you will be unable to make changes later.  Click `Continue`.

![Backend repo](/documentation/images/cicd-k8s/5-bringYourOwnAppCiBackend.png)

The CI pipeline stores some of its output to an Inventory repository, which will be stored in your IBM Cloud GitLab account (all IBM Cloud users are provided access to GitLab).

Name the Inventory repository (N.B as this repo will be used by multiple toolchains, you may wish to use a more generic name than shown in the screenshot, e.g. "compliance-inventory" rather than "compliannce-inventory-ci-backend") and click `Continue`:

![Inventory repo](/documentation/images/cicd-k8s/6-createInventoryRepocIBackend.png)

Similarly, name the Issues repo and click `Continue`:

![Issues rep](/documentation/images/cicd-k8s/7-createIssuesRepocIBackend.png)

Accept the default recommended Secrets Manager and click `Continue`

![Select Secrets Manager](/documentation/images/cicd-k8s/8-createSecretsManager.png)

Configure an existing Secrets Manager service by typing its instance name and resource group, then click `Continue`

![Configure Secrets Manager](/documentation/images/cicd-k8s/9-configureSecretsManager.png)

Name the Evidence storage repostitory.  The COS bucket can be added later if required, so leave this unchecked for now.  Click `Continue`:

![Configure Secrets Manager](/documentation/images/cicd-k8s/10-configureEvidenceStorage.png)

Accept the default Tekton pipeline definitions from the IBM repository.  This ensures your toolchain stays up to date with the latest additions.

![Use existing Tekton pipelines](/documentation/images/cicd-k8s/11-useExistingTektonPipelines.png)

Create a new API key (or use an existing one) for the toolchain to access your IBM Cloud account.  Specify the details of an existing IBM Cloud Container Registry (you may need to type this manually).  Specify the details of an existing IBM Cloud Kubernetes Service (IKS) cluster.  Click `Continue`:

![Registry and cluster config](/documentation/images/cicd-k8s/12-deploymentTarget.png)

Create a new Image Signing Key by speifying a name and email.  Click `Continue`:

![Signing key config1](/documentation/images/cicd-k8s/13-signingKey1.png)
![Signing key config2](/documentation/images/cicd-k8s/14-signingKey2.png)

DevOps Insights is automatically part of the toolchain being created, so select this instance and click `Continue`:

![DevOpsInsights config](/documentation/images/cicd-k8s/15-devOpsInsights.png)

SonarQube is automatically part of the toolchain being created, so select this instance and click `Continue`:

![SonarQube config](/documentation/images/cicd-k8s/16-sonarQube.png)

Select optional tools and click `Continue`:

![Optional tools](/documentation/images/cicd-k8s/17-optionalTools.png)

Review the summary and click `Create`:

![Summary](/documentation/images/cicd-k8s/18-summary.png)

After a few moments, the toolchain is displayed:

![Toolchain created](/documentation/images/cicd-k8s/18-toolchainCreated.png)

Click the CI pipeline, then `Environment properties`.  

![Environment Props](/documentation/images/cicd-k8s/19-environmentProps.png)

Add or update the Environmental Properties so they look like thise (using your own GitHub multi-tenancy/backend/frontend repos):

![Env props1](/documentation/images/cicd-k8s/ciBackendEnvProperties-1.png)
![Env props2](/documentation/images/cicd-k8s/ciBackendEnvProperties-1.png)


Select the `Trigger` tab.  You may need to correct the Git CI Trigger if it shows a hazard symbol.  Edit its properties and select a valid the branch name.

## Create CI toolchain for frontend

Repeat the above steps to create a second toolchain for frontend.  Use the following screenshots for guidance.

![CI Frontend 1](/documentation/images/cicd-k8s/cifrontend-1.png)
![CI Frontend 2](/documentation/images/cicd-k8s/cifrontend-2.png)
![CI Frontend 3](/documentation/images/cicd-k8s/cifrontend-3.png)
![CI Frontend 4](/documentation/images/cicd-k8s/cifrontend-4.png)
![CI Frontend 5](/documentation/images/cicd-k8s/cifrontend-5.png)
![CI Frontend 6](/documentation/images/cicd-k8s/cifrontend-6.png)
![CI Frontend 7](/documentation/images/cicd-k8s/cifrontend-7.png)
![CI Frontend 8](/documentation/images/cicd-k8s/cifrontend-8.png)
![CI Frontend 9](/documentation/images/cicd-k8s/cifrontend-9.png)
![CI Frontend 10](/documentation/images/cicd-k8s/cifrontend-10.png)
![CI Frontend 11](/documentation/images/cicd-k8s/cifrontend-11.png)
![CI Frontend 12](/documentation/images/cicd-k8s/cifrontend-12.png)
![CI Frontend 13](/documentation/images/cicd-k8s/cifrontend-13.png)
![CI Frontend 14](/documentation/images/cicd-k8s/cifrontend-14.png)
![CI Frontend 15](/documentation/images/cicd-k8s/cifrontend-15.png)
![CI Frontend 16](/documentation/images/cicd-k8s/cifrontend-16.png)
![CI Frontend 17](/documentation/images/cicd-k8s/cifrontend-17.png)
![CI Frontend 18](/documentation/images/cicd-k8s/cifrontend-18.png)

Add or update the Environmental Properties so they look like thise (using your own GitHub multi-tenancy/backend/frontend repos):

![Env props1](/documentation/images/cicd-k8s/ciFrontendEnvProperties-1.png)
![Env props2](/documentation/images/cicd-k8s/ciFrontendEnvProperties-1.png)

Note that for properties with type `Tool Integration`, also set JSON filter to `parameters.repo_url`

Select the `Trigger` tab.  You may need to correct the Git CI Trigger if it shows a hazard symbol.  Edit its properties and select a valid the branch name.

## Testing the CI pipelines

For both CI pipelines, enable a Dev mode trigger which permits a faster pipeline run which does not invoke all compliance steps.  Select the CI pipeline tile, then Trigger.  Duplicate the existing Manual Trigger:

![Duplicate manual trigger](/documentation/images/cicd-k8s/29-duplicateManualTrigger.png)

![Create dev mode trigger](/documentation/images/cicd-k8s/30-createDevModeTrigger.png)

First test the backend CI pipelines by triggering a pipeline run, using dev mode:

![Pipeline run trigger](/documentation/images/cicd-k8s/31-testCiBackendDevMode.png)

Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

![Pipeline run success](/documentation/images/cicd-k8s/32-cIBackendDevModeSuccess.png)

Test the front end CI pipeline by triggering a pipeline run, using dev mode:

Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

TODO