# Create DevSecOps pipelines to update a Kubernetes application 

DevSecOps integrates a set of security and compliance controls into the DevOps processes, allowing frequent code delivery while maintaining a strong security posture and continuous state of audit-readiness.

This setion steps you through the creation of Continuous Integration (CD) toolchains using an IBM Cloud toolchain templates, which we will customize to deploy the sample application to an IBM Cloud Kuberenets cluster.

## Before you begin

Set up the following pre-requisites:

- Ensure you have cloned the multitenancy-backend and multitenancy-frontend repos to your GitHub account
- An instance of the IBM Cloud Secrets Manager service
- An IBM Cloud Kubernetes Service cluster or An IBM Cloud OpenShift cluster
- Create a namespace in the IBM Cloud Container Registry (access via hamburger menu->Container Registry)
- A IBM Cloud Object Storage (COS) service instance, and a bucket to store data

## Create the toolchain

login to IBM Cloud and use the hamburger menu in the top left to navigate to `DevOps`.  Select the Resource Group and Region:

![DevSecOps](/documentation/images/cicd-k8s/1-devOpsSelectRegion.png)

Clieck `Create Toolchain` and select the `DevSecOps` filter:

![DevSecOpsFiltered](/documentation/images/cicd-k8s/2-filterToDevSecOpsToolchains.png)

## Create CD toolchain

Click the CD tile to launch the setup wizard:

![CD Setup Wizard](/documentation/images/cicd-k8s/3-cISetupWizard.png)

Name the toolchain and click `Continue`:

![Name CD Toolchain](/documentation/images/cicd-k8s/22-cdPipeline.png)

Select the Inventory repository, based on the repository used in the CI pipeline and click `Continue`:

![Inventory repo](/documentation/images/cicd-k8s/23-createInventoryRepocDBackend.png)

Select the Issues repository, based on the repository used in the CI pipeline and click `Continue`:

![Issues repo](/documentation/images/cicd-k8s/24-createIssuesRepocDBackend.png)

Select the repository for Pipeline configuration, Use the multi-tenancy repository which you cloned to your GitHub account.  Do not use the IBM repository as pictured below, as you will be unable to make changes later.  Click `Continue`.

![Pipeline config repo](/documentation/images/cicd-k8s/25-pipelineConfigCd.png)

Click `Continue` to use the Secrets Manager service:

![Pipeline config repo](/documentation/images/cicd-k8s/26-secretsManagerCd.png)

Specify the connection details for your secrets manager:

![Secrets manager](/documentation/images/cicd-k8s/27-secretsManagerConfigCd.png)

Use the default Evidence Storage repository, based on the repository used in the CI pipeline and click `Continue`:

![Evidence Storage](/documentation/images/cicd-k8s/28-evidenceStorageCd.png)

Configure the toolchain to connect to a COS bucket.  If you haven't created this, the toolchain provides a link to detailed instructions.  The value for `Endpoint` can be found from the COS service, in the `Endpoints` section, where you should select the `Resilency` and `Location` of your COS bucket and copy the private endpoint URL.  

The toolchain wizard also allows you to create a new `Service API key` allowing the toolchain to write to your COS bucket.  Click `Continue`:

![COS config](/documentation/images/cicd-k8s/33-cosCdPipeline.png)
![COS config](/documentation/images/cicd-k8s/34-cosCdPipelineAPI.png)

Use the default Tekton pipeline and click `Continue`:

![Tekton pipeline](/documentation/images/cicd-k8s/35-tektonPipelineCd.png)

Create a new API key for the CD pipeline and save it to the Secrets Manager.  Select the `Resource Group` and `Cluster Name` and leave other fields to their defaults.  Click `Continue`:

![API Key](/documentation/images/cicd-k8s/36-apiKeyCdPipeline.png)
![API Key](/documentation/images/cicd-k8s/37-cdDeploymentTarget.png)

Accept the defaults for Change Request Management and click `Continue`:

![Change Management](/documentation/images/cicd-k8s/38-changeRequestManagement.png)

Accept the defaults for DevOpsInsights toolchain and click `Continue`:

![Devops Insights](/documentation/images/cicd-k8s/39-devopsInsights.png)

Configure the optional tools as follows:

![Configure tools](/documentation/images/cicd-k8s/40-cdTools.png)

The Security and Compliance Center requires a repository.  Data from the CI and CD pipelines can be stored in the same repository, so reuse the repository created previously in the CI toolchain for evidence storage.  You click the link to the `Evidence Storage` step to copy the URL.

![Configure Security Compliance](/documentation/images/cicd-k8s/41-securityAndCompliance.png)

Create the CD toolchain by pressing the `Create` button:

![Configure Security Compliance](/documentation/images/cicd-k8s/42-createCdToolchain.png)

After a few moments the toolchain will be ready:

![Toolchain Ready](/documentation/images/cicd-k8s/43-cdToolchainReady.png)

The environmental properties for the frontend CD pipeline should be updated as follows:

Add or update the following properties:

| Key  | Value | Type |
| ------------- | ------------- | ------------- |
| branch  | `main`  | Text |
| pipeline-config-branch | update from `master` to `main`  | Text |
| repository | select your repo, e.g. `https://github.com/<your-org>/multi-tenancy` from the list.  Also set JSON filter to `parameters.repo_url`  | Tool Integration |
| tenant  | `tenant`  | Text |


You may need to correct the Git CD Trigger:

![Git CD Trigger error](/documentation/images/cicd-k8s/44-gitCdTriggerError.png)

Edit its properties and select a valid the branch name:

![Correct CD Trigger error](/documentation/images/cicd-k8s/45-correctGitTriggerBranch.png)

Also, copy the existing `Manual CD Trigger` and use it to create a `Manual CD Trigger force redeploy` trigger which can be used to bypass some elements of the toolchain, for testing purposes.  Note that two properties have been added:

![Correct CD Trigger error](/documentation/images/cicd-k8s/46-forceRedoployTrigger.png)

| Key  | Value | Type |
| ------------- | ------------- | ------------- |
| force-redeploy | `true`  | Text |
| repository | select your repo, e.g. `https://github.com/<your-org>/multi-tenancy` from the list.  Also set JSON filter to `parameters.repo_url`  |

## Testing the CD pipelines

If you have successfully executed the CI pipelines for backend and frontend, you can test the CD pipeline:

![Correct CD Trigger error](/documentation/images/cicd-k8s/47-manualCdTrigger.png)

Click the pipeline run name to view the progress.  After a few minutes, a successful result should look like this:

WORK IN PROGRESS
